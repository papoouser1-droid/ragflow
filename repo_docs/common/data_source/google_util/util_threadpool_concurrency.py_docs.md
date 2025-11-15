# File Documentation: common/data_source/google_util/util_threadpool_concurrency.py

## File Metadata

- **Path**: `common/data_source/google_util/util_threadpool_concurrency.py`
- **Extension**: `.py`
- **Lines**: 142
- **Characters**: 4,855
- **Size**: 4,855 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

    """

## Detailed Walkthrough

### Classes (1)

- `ThreadSafeDict`: Class definition

### Imports (7)

- `import collections.abc`
- `import copy`
- `import threading`
- `from collections.abc import Callable, Iterator, MutableMapping`
- `from typing import Any, TypeVar, overload`
- `from pydantic import GetCoreSchemaHandler`
- `from pydantic_core import core_schema`

## Code Structure Analysis

- Total lines: 142
- Blank lines: 32 (22.5%)
- Comment lines: ~17 (12.0%)
- Code lines: ~93


## Dependencies and Imports

- `import collections.abc`
- `import copy`
- `import threading`
- `from collections.abc import Callable, Iterator, MutableMapping`
- `from typing import Any, TypeVar, overload`
- `from pydantic import GetCoreSchemaHandler`
- `from pydantic_core import core_schema`

## Design & Architecture

This file is located in the `common` directory, specifically within `common/data_source/google_util`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/data_source/google_util/` directory
- Imports from `collections.abc`
- Imports from `pydantic`
- Imports from `pydantic_core`
- Potential test file: `test_util_threadpool_concurrency.py`

## Keywords

Any, Basic, Bulk, Callable, CoreSchema, Create, Default, Example, Get, GetCoreSchemaHandler, Implements, ItemsView, Iterator, Key, KeysView, Lock, MutableMapping, None, Python, Remove, Replace, Return, Returns, Set, ThreadSafeDict, TypeVar, Update, VT, Value, ValuesView, _T, __deepcopy__, __delitem__, __get_pydantic_core_schema__, __getitem__, __init__, __iter__, __len__, __setitem__, applied, atomic_get_set, class, classmethod, clear, copy, get, items, keys, overload, pop...

---
*Generated by RAGFlow Repository Documentation Generator*
