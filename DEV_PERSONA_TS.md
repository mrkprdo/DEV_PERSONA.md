# DEV_PERSONA_TS - https://github.com/mrkprdo/DEV_PERSONA.md

TypeScript 5.0+ development standards and conventions for consistent, maintainable code.

## Language Standard

- **TypeScript 5.0+** with `strict: true`
- **Target**: `ES2020` minimum
- `moduleResolution: "node"`

## File Naming & Extensions

| Type       | Extension | Convention       |
| ---------- | --------- | ---------------- |
| Source     | `.ts`     | `snake_case.ts`  |
| Component  | `.tsx`    | `PascalCase.tsx` |
| Type defs  | `.ts`     | `types.ts`       |
| Config     | `.json`   | `tsconfig.json`  |

## Indentation & Braces

- **2 spaces** (JavaScript community standard)
- **Max line length**: 100 characters
- Brace on same line (K&R style)

```typescript
function processSensor(data: SensorData, timeoutMs: number): boolean {
  if (data.status === "ready") {
    return handleData(data);
  } else {
    return false;
  }
}
```

## Naming Conventions

| Category              | Pattern       | Example                      |
| --------------------- | ------------- | ---------------------------- |
| Function / Method     | `camelCase`   | `readSensor()`, `processData()` |
| Class / Type / Enum   | `PascalCase`  | `class SensorDriver`, `type SensorData` |
| Constant              | `UPPER_SNAKE` | `MAX_RETRIES`, `DEFAULT_TIMEOUT_MS` |
| Variable (local)      | `camelCase`   | `temperature`, `bufferSize` |
| Variable (module)     | `camelCase`   | `privateCache`, `publicConfig` |
| Private method        | `_camelCase`  | `_validateChecksum()` |
| Interface             | `IPascalCase` | `interface ISensorDriver` (optional `I` prefix) |
| Type alias            | `PascalCase`  | `type SensorReading = {...}` |

## Comments & JSDoc

**Required for all public exports** — TSDoc/JSDoc format:

```typescript
/**
 * Read sensor data from I2C device.
 *
 * @param address - 7-bit I2C slave address.
 * @param timeoutMs - Timeout in milliseconds. Defaults to 100.
 * @returns Sensor reading with temperature, humidity, and status.
 * @throws {SensorTimeoutError} If I2C read exceeds timeout.
 * @throws {SensorNotFoundError} If device at address does not respond.
 * 
 * @example
 * const data = await readSensor(0x48, 100);
 * console.log(data.temperature);
 */
export async function readSensor(
  address: number,
  timeoutMs: number = 100,
): Promise<SensorData> {
```

**Inline comments**: only for non-obvious logic — explain WHY, not WHAT.

## Module Structure

```typescript
// === types / interfaces ===

export interface SensorData {
  temperature: number;
  humidity: number;
  status: "ready" | "error" | "busy";
}

export enum SensorState {
  IDLE = "idle",
  READING = "reading",
  ERROR = "error",
}

// === constants ===

export const MAX_RETRIES = 3;
export const DEFAULT_TIMEOUT_MS = 100;

// === public exports ===

export class SensorDriver {
  private address: number;
  private handle: I2cDriver | null = null;

  constructor(address: number) {
    this.address = address;
  }

  /**
   * Initialize I2C connection to sensor.
   * @returns true if initialization succeeded.
   */
  async init(): Promise<boolean> {
    // implementation
  }

  /**
   * Read latest sensor values.
   * @throws {Error} If driver not initialized.
   */
  async read(): Promise<SensorData> {
    // implementation
  }

  private validateChecksum(data: Uint8Array): boolean {
    // private helper
  }
}

// === private helpers (if any) ===

function formatPayload(data: SensorData): string {
  // implementation
}
```

## TypeScript 5+ Features — Prefer These

- Strict null checking: `?:` for optional, `|` for unions
- Type inference where obvious: `const x = 5` (type is `number`)
- `as const` for literal types in const objects
- `satisfies` operator for type constraints
- Discriminated unions for state machines
- Async/await over `.then()` chains
- Type predicates: `function isSensor(x: unknown): x is SensorData`
- `Record<K, V>` for maps, not `{[key: string]: T}`
- Enums only for string unions; prefer `type Status = "ready" | "error"`
- Decorators (if supported) for metadata
- Arrow functions for lambdas, `function` for named exports

## Prohibited

- `any` type (use `unknown` + type guard if truly dynamic)
- `as any` casts — be explicit with proper types
- C-style naming (lValue, mMember prefixes)
- Implicit `undefined` returns — always type them
- `require()` in modern code — use `import`
- Magic numbers without constants
- Nested ternaries — use `if`/`else` or `match`-style dispatch
- `var` — use `const` / `let`
- Async functions without proper error handling

## React Conventions (if applicable)

- Component files: `PascalCase.tsx`
- Props interface: `${ComponentName}Props`
- Hooks: custom hooks named `use${Feature}`
- Memoization: use `React.memo` for expensive re-renders
- State management: `useState` + context, or external (Redux/Zustand)
- Type all props: no implicit `any`

## File Header Template

New TypeScript/JavaScript files must include this banner at the top:

```typescript
/**
 * ============================================================================
 * Author: Mark Anthony Prado
 * Created: [YYYY-MM-DD]
 * ============================================================================
 */
```

Example (`sensor-driver.ts`):

```typescript
/**
 * ============================================================================
 * Author: Mark Anthony Prado
 * Created: 2026-07-08
 * ============================================================================
 */

import { I2cDriver } from "./i2c-driver";

// rest of file...
```

React component example (`SensorCard.tsx`):

```typescript
/**
 * ============================================================================
 * Author: Mark Anthony Prado
 * Created: 2026-07-08
 * ============================================================================
 */

import React from "react";

export const SensorCard: React.FC<SensorCardProps> = (props) => {
  // component code...
};
```

## Testing

- Framework: `Jest` or `Vitest`
- Test file naming: `*.test.ts` or `*.spec.ts` in `__tests__/` or colocated
- Use type-safe mocks: `jest.mock()` with proper typing
- Test async code: `async/await`, not callbacks
