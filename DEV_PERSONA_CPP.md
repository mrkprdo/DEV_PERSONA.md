# DEV_PERSONA_CPP - https://github.com/mrkprdo/DEV_PERSONA.md

C++23 development standards and conventions for consistent, maintainable code.

## Language Standard

- **C++23** — use `-std=c++23` / `/std:c++latest`

## File Naming & Extensions

| Type   | Extension | Convention       |
| ------ | --------- | ---------------- |
| Source | `.cpp`    | `snake_case.cpp` |
| Header | `.hpp`    | `snake_case.hpp` |

## Indentation & Braces

- **Allman style**: brace on its own line at same indent, content indented one level
- Indent: **4 spaces** (no tabs)

```cpp
void Bar()
{
    if (lCondition)
    {
        DoThing();
    }
    else
    {
        DoOther();
    }
}
```

## Naming Conventions

| Category              | Prefix | Pattern       | Example                                  |
| --------------------- | ------ | ------------- | ---------------------------------------- |
| Local variable        | `l`    | `lPascalCase` | `lTemperature`, `lBufferSize`            |
| Function argument     | `a`    | `aPascalCase` | `aSensorData`, `aTimeoutMs`              |
| Member variable       | `m`    | `mPascalCase` | `mSensorHandle`, `mBuffer`               |
| Static variable       | `s`    | `sPascalCase` | `sInstanceCount`, `sPool`                |
| Constant / Enum value | —      | `UPPER_SNAKE` | `MAX_RETRIES`, `GPIO_PIN_5`              |
| Function / Method     | —      | `PascalCase`  | `ReadSensor()`, `ProcessPacket()`        |
| Class / Struct / Enum | —      | `PascalCase`  | `class SensorDriver`, `enum class State` |
| Namespace             | —      | `snake_case`  | `namespace sensor::hal`                  |
| Template param        | —      | `PascalCase`  | `template<typename T>`                   |

### Variable name rules

1. `constexpr` / `const` at file/namespace scope → `UPPER_SNAKE`
2. `const` local → `lPascalCase`
3. Member variables ALWAYS use `m` prefix inside class definition
4. Static members use `s` prefix

## Comments

- **`.cpp` files**: ZERO comments. No inline, no block, no doc comments. Code must be self-documenting through naming and structure.
- **`.hpp` files**: Full Doxygen documentation for every declaration:

```cpp
/**
 * @brief One-line summary.
 *
 * Detailed description with pre/post conditions, thread safety,
 * and parameter semantics.
 *
 * @param aParamName  Description of the parameter.
 * @return Description of the return value.
 * @throws Description of any exceptions.
 */
void Foo(int aParamName);
```

### Doxygen requirements (hpp)

- `@brief`, `@param` (each parameter), `@return` on every public function
- Class-level `@brief` describing ownership, lifetime, thread safety
- Enum/constant: `@brief` + `@note` if bitwise or special semantics
- Omit redundant `@param`/`@return` only for trivial getters (single field return)

## Header Structure

```cpp
#pragma once

// === includes ===
//   standard first, then third-party, then project, alphabetical within groups
#include <cstdint>
#include <vector>

#include "driver/i2c.hpp"
#include "sensor_types.hpp"

// === forward declarations ===

// === constants ===
constexpr uint32_t I2C_TIMEOUT_MS = 100;

// === class / struct ===
class SensorDriver
{
public:
    // ---- public interface ----
    SensorDriver() = default;
    ~SensorDriver() = default;

    SensorDriver(SensorDriver const&) = delete;            // no copy
    SensorDriver& operator=(SensorDriver const&) = delete;
    SensorDriver(SensorDriver&&) = default;                // movable
    SensorDriver& operator=(SensorDriver&&) = default;

    /**
     * @brief Initialise sensor with given I2C address.
     * @param aAddress  7-bit I2C slave address.
     * @return true if initialisation succeeded.
     */
    [[nodiscard]] bool Init(uint8_t aAddress);

private:
    int mI2cHandle = -1;
    static int sOpenCount;
};
```

## C++23 Features — Prefer These

- `std::expected` over exceptions for expected failures
- `std::optional` for maybe-values
- `std::span` over raw pointer+length
- `[[nodiscard]]`, `[[maybe_unused]]`, `[[likely]]`, `[[unlikely]]`
- `constexpr` / `consteval` where possible
- `using` over `typedef`
- `enum class` over plain `enum`
- Structured bindings
- `std::mdspan` for multi-dimensional access
- `std::print` over `printf`/`cout`
- `auto` for variables when type is obvious from right-hand side

## File Header Template

New source and header files must include this banner at the top:

```cpp
/**
 * ============================================================================
 *  Author: Mark Anthony Prado
 *  Created: [YYYY-MM-DD]
 * ============================================================================
 */
```

Example (`sensor_driver.hpp`):

```cpp
/**
 * ============================================================================
 *  Author: Mark Anthony Prado
 *  Created: 2026-07-08
 * ============================================================================
 */

#pragma once

#include <cstdint>
#include <vector>

// rest of file...
```

## Prohibited

- `malloc` / `free` in application code
- C-style casts (`(int)x`) — use `static_cast<>`, `reinterpret_cast<>`
- VLAs (Variable Length Arrays)
- `#pragma once` is acceptable (the only exception to the "prohibited preprocessor" rule); otherwise prefer `#ifndef` guards for maximum portability
- `goto` (except in device-specific error cleanup, documented in hpp)
- Hungarian notation beyond the prefix table above
