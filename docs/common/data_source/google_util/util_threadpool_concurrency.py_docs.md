# Documentation: common/data_source/google_util/util_threadpool_concurrency.py

## File Metadata

- **Path**: `common/data_source/google_util/util_threadpool_concurrency.py`
- **Size**: 4855 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/google_util/util_threadpool_concurrency.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `collections.abc`
- `copy`
- `threading`
- `collections.abc`
- `typing`
- `pydantic`
- `pydantic_core`

### Classes Defined

This file defines 1 class(es):

#### Class: `ThreadSafeDict` (line 16)

**Docstring**: A thread-safe dictionary implementation that uses a lock to ensure thread safety.
Implements the MutableMapping interface to provide a complete dictionary-like interface.

Example usage:
    # Create ...

**Methods**: __init__, __getitem__, __setitem__, __delitem__, __iter__, __len__, __get_pydantic_core_schema__, validate, __deepcopy__, clear, copy, get, get, get, pop, setdefault, update, items, keys, values, atomic_get_set, atomic_get_set, atomic_get_set

### Functions Defined

This file defines 23 function(s):

#### Function: `__init__` (line 34)

**Parameters**: self, input_dict

#### Function: `__getitem__` (line 38)

**Parameters**: self, key

#### Function: `__setitem__` (line 42)

**Parameters**: self, key, value

#### Function: `__delitem__` (line 46)

**Parameters**: self, key

#### Function: `__iter__` (line 50)

**Parameters**: self

#### Function: `__len__` (line 55)

**Parameters**: self

#### Function: `__get_pydantic_core_schema__` (line 60)

**Parameters**: cls, source_type, handler

#### Function: `validate` (line 64)

**Parameters**: cls, v

#### Function: `__deepcopy__` (line 69)

**Parameters**: self, memo

#### Function: `clear` (line 72)

**Parameters**: self

**Docstring**: Remove all items from the dictionary atomically....

#### Function: `copy` (line 77)

**Parameters**: self

**Docstring**: Return a shallow copy of the dictionary atomically....

#### Function: `get` (line 83)

**Parameters**: self, key

#### Function: `get` (line 86)

**Parameters**: self, key, default

#### Function: `get` (line 88)

**Parameters**: self, key, default

**Docstring**: Get a value with a default, atomically....

#### Function: `pop` (line 93)

**Parameters**: self, key, default

**Docstring**: Remove and return a value with optional default, atomically....

#### Function: `setdefault` (line 100)

**Parameters**: self, key, default

**Docstring**: Set a default value if key is missing, atomically....

#### Function: `update` (line 105)

**Parameters**: self

**Docstring**: Update the dictionary atomically from another mapping or from kwargs....

#### Function: `items` (line 110)

**Parameters**: self

**Docstring**: Return a view of (key, value) pairs atomically....

#### Function: `keys` (line 115)

**Parameters**: self

**Docstring**: Return a view of keys atomically....

#### Function: `values` (line 120)

**Parameters**: self

**Docstring**: Return a view of values atomically....

#### Function: `atomic_get_set` (line 126)

**Parameters**: self, key, value_callback, default

#### Function: `atomic_get_set` (line 129)

**Parameters**: self, key, value_callback, default

#### Function: `atomic_get_set` (line 131)

**Parameters**: self, key, value_callback, default

**Docstring**: Replace a value from the dict with a function applied to the previous value, atomically.

Returns:
    A tuple of the previous value and the new value....

## Original Source Code

```py
import collections.abc
import copy
import threading
from collections.abc import Callable, Iterator, MutableMapping
from typing import Any, TypeVar, overload

from pydantic import GetCoreSchemaHandler
from pydantic_core import core_schema

R = TypeVar("R")
KT = TypeVar("KT")  # Key type
VT = TypeVar("VT")  # Value type
_T = TypeVar("_T")  # Default type


class ThreadSafeDict(MutableMapping[KT, VT]):
    """
    A thread-safe dictionary implementation that uses a lock to ensure thread safety.
    Implements the MutableMapping interface to provide a complete dictionary-like interface.

    Example usage:
        # Create a thread-safe dictionary
        safe_dict: ThreadSafeDict[str, int] = ThreadSafeDict()

        # Basic operations (atomic)
        safe_dict["key"] = 1
        value = safe_dict["key"]
        del safe_dict["key"]

        # Bulk operations (atomic)
        safe_dict.update({"key1": 1, "key2": 2})
    """

    def __init__(self, input_dict: dict[KT, VT] | None = None) -> None:
        self._dict: dict[KT, VT] = input_dict or {}
        self.lock = threading.Lock()

    def __getitem__(self, key: KT) -> VT:
        with self.lock:
            return self._dict[key]

    def __setitem__(self, key: KT, value: VT) -> None:
        with self.lock:
            self._dict[key] = value

    def __delitem__(self, key: KT) -> None:
        with self.lock:
            del self._dict[key]

    def __iter__(self) -> Iterator[KT]:
        # Return a snapshot of keys to avoid potential modification during iteration
        with self.lock:
            return iter(list(self._dict.keys()))

    def __len__(self) -> int:
        with self.lock:
            return len(self._dict)

    @classmethod
    def __get_pydantic_core_schema__(cls, source_type: Any, handler: GetCoreSchemaHandler) -> core_schema.CoreSchema:
        return core_schema.no_info_after_validator_function(cls.validate, handler(dict[KT, VT]))

    @classmethod
    def validate(cls, v: Any) -> "ThreadSafeDict[KT, VT]":
        if isinstance(v, dict):
            return ThreadSafeDict(v)
        return v

    def __deepcopy__(self, memo: Any) -> "ThreadSafeDict[KT, VT]":
        return ThreadSafeDict(copy.deepcopy(self._dict))

    def clear(self) -> None:
        """Remove all items from the dictionary atomically."""
        with self.lock:
            self._dict.clear()

    def copy(self) -> dict[KT, VT]:
        """Return a shallow copy of the dictionary atomically."""
        with self.lock:
            return self._dict.copy()

    @overload
    def get(self, key: KT) -> VT | None: ...

    @overload
    def get(self, key: KT, default: VT | _T) -> VT | _T: ...

    def get(self, key: KT, default: Any = None) -> Any:
        """Get a value with a default, atomically."""
        with self.lock:
            return self._dict.get(key, default)

    def pop(self, key: KT, default: Any = None) -> Any:
        """Remove and return a value with optional default, atomically."""
        with self.lock:
            if default is None:
                return self._dict.pop(key)
            return self._dict.pop(key, default)

    def setdefault(self, key: KT, default: VT) -> VT:
        """Set a default value if key is missing, atomically."""
        with self.lock:
            return self._dict.setdefault(key, default)

    def update(self, *args: Any, **kwargs: VT) -> None:
        """Update the dictionary atomically from another mapping or from kwargs."""
        with self.lock:
            self._dict.update(*args, **kwargs)

    def items(self) -> collections.abc.ItemsView[KT, VT]:
        """Return a view of (key, value) pairs atomically."""
        with self.lock:
            return collections.abc.ItemsView(self)

    def keys(self) -> collections.abc.KeysView[KT]:
        """Return a view of keys atomically."""
        with self.lock:
            return collections.abc.KeysView(self)

    def values(self) -> collections.abc.ValuesView[VT]:
        """Return a view of values atomically."""
        with self.lock:
            return collections.abc.ValuesView(self)

    @overload
    def atomic_get_set(self, key: KT, value_callback: Callable[[VT], VT], default: VT) -> tuple[VT, VT]: ...

    @overload
    def atomic_get_set(self, key: KT, value_callback: Callable[[VT | _T], VT], default: VT | _T) -> tuple[VT | _T, VT]: ...

    def atomic_get_set(self, key: KT, value_callback: Callable[[Any], VT], default: Any = None) -> tuple[Any, VT]:
        """Replace a value from the dict with a function applied to the previous value, atomically.

        Returns:
            A tuple of the previous value and the new value.
        """
        with self.lock:
            val = self._dict.get(key, default)
            new_val = value_callback(val)
            self._dict[key] = new_val
            return val, new_val

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/google_util/util_threadpool_concurrency.py` is located in the `common/data_source/google_util` directory.

### Architecture Context

Files in this location typically handle concerns related to google_util.

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
- [auth.py](auth.py_docs.md)
- [constant.py](constant.py_docs.md)
- [oauth_flow.py](oauth_flow.py_docs.md)
- [resource.py](resource.py_docs.md)
- [util.py](util.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
