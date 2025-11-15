# Documentation: api/utils/json_encode.py

## File Metadata

- **Path**: `api/utils/json_encode.py`
- **Size**: 2512 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/utils/json_encode.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `json`
- `enum`
- `api.utils.common`

### Classes Defined

This file defines 2 class(es):

#### Class: `BaseType` (line 7)

**Methods**: to_dict, to_dict_with_type

#### Class: `CustomJSONEncoder` (line 36)

**Methods**: __init__, default

### Functions Defined

This file defines 7 function(s):

#### Function: `json_dumps` (line 63)

**Parameters**: src, byte, indent, with_type

#### Function: `json_loads` (line 74)

**Parameters**: src, object_hook, object_pairs_hook

#### Function: `to_dict` (line 8)

**Parameters**: self

#### Function: `to_dict_with_type` (line 11)

**Parameters**: self

#### Function: `__init__` (line 37)

**Parameters**: self

#### Function: `default` (line 41)

**Parameters**: self, obj

#### Function: `_dict` (line 12)

**Parameters**: obj

## Original Source Code

```py
import datetime
import json
from enum import Enum, IntEnum
from api.utils.common import string_to_bytes, bytes_to_string


class BaseType:
    def to_dict(self):
        return dict([(k.lstrip("_"), v) for k, v in self.__dict__.items()])

    def to_dict_with_type(self):
        def _dict(obj):
            module = None
            if issubclass(obj.__class__, BaseType):
                data = {}
                for attr, v in obj.__dict__.items():
                    k = attr.lstrip("_")
                    data[k] = _dict(v)
                module = obj.__module__
            elif isinstance(obj, (list, tuple)):
                data = []
                for i, vv in enumerate(obj):
                    data.append(_dict(vv))
            elif isinstance(obj, dict):
                data = {}
                for _k, vv in obj.items():
                    data[_k] = _dict(vv)
            else:
                data = obj
            return {"type": obj.__class__.__name__,
                    "data": data, "module": module}

        return _dict(self)


class CustomJSONEncoder(json.JSONEncoder):
    def __init__(self, **kwargs):
        self._with_type = kwargs.pop("with_type", False)
        super().__init__(**kwargs)

    def default(self, obj):
        if isinstance(obj, datetime.datetime):
            return obj.strftime('%Y-%m-%d %H:%M:%S')
        elif isinstance(obj, datetime.date):
            return obj.strftime('%Y-%m-%d')
        elif isinstance(obj, datetime.timedelta):
            return str(obj)
        elif issubclass(type(obj), Enum) or issubclass(type(obj), IntEnum):
            return obj.value
        elif isinstance(obj, set):
            return list(obj)
        elif issubclass(type(obj), BaseType):
            if not self._with_type:
                return obj.to_dict()
            else:
                return obj.to_dict_with_type()
        elif isinstance(obj, type):
            return obj.__name__
        else:
            return json.JSONEncoder.default(self, obj)


def json_dumps(src, byte=False, indent=None, with_type=False):
    dest = json.dumps(
        src,
        indent=indent,
        cls=CustomJSONEncoder,
        with_type=with_type)
    if byte:
        dest = string_to_bytes(dest)
    return dest


def json_loads(src, object_hook=None, object_pairs_hook=None):
    if isinstance(src, bytes):
        src = bytes_to_string(src)
    return json.loads(src, object_hook=object_hook,
                      object_pairs_hook=object_pairs_hook)

```

## Detailed Analysis

### File Role in Repository

The file `api/utils/json_encode.py` is located in the `api/utils` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to utils.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Ensure all user inputs are validated
- Check for SQL injection vulnerabilities
- Verify authentication and authorization

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [__init__.py](__init__.py_docs.md)
- [api_utils.py](api_utils.py_docs.md)
- [base64_image.py](base64_image.py_docs.md)
- [commands.py](commands.py_docs.md)
- [common.py](common.py_docs.md)
- [configs.py](configs.py_docs.md)
- [crypt.py](crypt.py_docs.md)
- [email_templates.py](email_templates.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [health_utils.py](health_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
