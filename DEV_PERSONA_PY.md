# DEV_PERSONA_PY - https://github.com/mrkprdo/DEV_PERSONA.md

Python 3.11+ development standards and conventions for consistent, maintainable code.

## Language Standard

- **Python 3.11+** — use type hints, `from __future__ import annotations`

## File Naming & Extensions

| Type      | Extension | Convention       |
| --------- | --------- | ---------------- |
| Module    | `.py`     | `snake_case.py`  |
| Package   | `dir`     | `snake_case/`    |
| Test      | `.py`     | `test_*.py`      |
| Config    | `.toml`   | `pyproject.toml` |

## Indentation & Braces

- **4 spaces** (PEP 8 standard, no tabs)
- **Max line length**: 100 characters
- Line continuations: parentheses/brackets preferred over backslash

```python
def process_sensor(
    sensor_data: dict[str, Any],
    timeout_ms: int,
) -> bool:
    if sensor_data["status"] == "ready":
        return handle_data(sensor_data)
    else:
        return False
```

## Naming Conventions

| Category            | Pattern       | Example                      |
| ------------------- | ------------- | ---------------------------- |
| Function / Method   | `snake_case`  | `read_sensor()`, `process_data()` |
| Class / Exception   | `PascalCase`  | `class SensorDriver`, `class SensorError` |
| Constant            | `UPPER_SNAKE` | `MAX_RETRIES`, `GPIO_PIN_5`  |
| Variable (local)    | `snake_case`  | `temperature`, `buffer_size` |
| Variable (module)   | `snake_case`  | `_private_cache`, `public_config` |
| Private method      | `_snake_case` | `_validate_checksum()` |
| Dunder method       | `__name__`    | `__init__()`, `__str__()` |
| Enum member         | `UPPER_SNAKE` | `class State(Enum): IDLE = 1` |

## Docstrings

**Required for all public functions, classes, and modules** — Google style:

```python
def read_sensor(address: int, timeout_ms: int = 100) -> SensorData:
    """Read sensor data from I2C device.
    
    Args:
        address: 7-bit I2C slave address.
        timeout_ms: Timeout in milliseconds.
    
    Returns:
        SensorData object with temperature, humidity, and status.
    
    Raises:
        SensorTimeoutError: If I2C read exceeds timeout.
        SensorNotFoundError: If device at address does not respond.
    """
```

**Inline comments**: only for non-obvious logic — explain WHY, not WHAT.

## Module Structure

```python
from __future__ import annotations

import logging
import sys
from typing import Any

import numpy as np

from .driver import I2cDriver
from .types import SensorData

logger = logging.getLogger(__name__)

# === constants ===
MAX_RETRIES = 3
DEFAULT_TIMEOUT_MS = 100

# === public functions / classes ===

class SensorDriver:
    """Manages I2C communication with sensor hardware.
    
    Thread-safe for concurrent reads. Each instance holds a single I2C handle.
    """
    
    def __init__(self, address: int) -> None:
        """Initialize driver for sensor at given address."""
        self._address = address
        self._handle: I2cDriver | None = None
    
    def __enter__(self) -> SensorDriver:
        """Context manager entry."""
        self.init()
        return self
    
    def __exit__(self, *args: Any) -> None:
        """Context manager exit."""
        self.close()
    
    def init(self) -> bool:
        """Open I2C connection and verify sensor is present."""
    
    def read(self) -> SensorData:
        """Read latest sensor values.
        
        Returns:
            SensorData with current readings.
        
        Raises:
            RuntimeError: If driver not initialized.
        """

# === private functions ===

def _validate_checksum(data: bytes) -> bool:
    """Verify CRC16 checksum on sensor payload."""
```

## Python 3.11+ Features — Prefer These

- `from __future__ import annotations` (enable PEP 563 everywhere)
- Type hints on all function signatures
- `dataclass` (or `pydantic.BaseModel` for validation)
- `Enum` / `StrEnum` for constants
- `logging` module over `print()`
- Context managers (`with` statement, `__enter__`/`__exit__`)
- `pathlib.Path` over `os.path`
- `match`/`case` (match statements) for dispatch
- `|` (union types) — `int | None` over `Optional[int]`
- f-strings over `.format()` or `%` formatting
- Type hints: `TypeVar`, `Protocol`, `Literal`, `TypedDict`

## Prohibited

- `eval()` / `exec()` (security risk)
- Untyped functions (missing type hints)
- Bare `except:` clauses — be specific
- Mutable default arguments: `def foo(x=[])` — use `None` + guard
- `from module import *` — import explicitly
- Hungarian notation (prefixes like `lValue`, `mMember`)
- C-style variable names in function scopes
- `global` keyword (use class state or pass as parameter)
- Tabs — 4 spaces always

## File Header Template

New Python files must include this banner at the top:

```python
"""
================================================================================
Author: Mark Anthony Prado
Created: [YYYY-MM-DD]
================================================================================
"""

from __future__ import annotations

# rest of imports and code...
```

Example (`sensor_driver.py`):

```python
"""
================================================================================
Author: Mark Anthony Prado
Created: 2026-07-08
================================================================================
"""

from __future__ import annotations

import logging
from typing import Any

logger = logging.getLogger(__name__)

# rest of file...
```

## Testing

- Unit tests: `pytest` with assertions, no `unittest.TestCase`
- Test file naming: `test_module_name.py` in `tests/` directory
- Use fixtures for setup/teardown
- Mock external I2C/sensor calls in unit tests, use real hardware in integration tests
