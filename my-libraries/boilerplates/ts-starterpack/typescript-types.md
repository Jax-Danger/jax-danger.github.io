# TypeScript Types for FiveM

This guide covers TypeScript types and type annotations specifically for FiveM development.

## Basic Types

### Primitive Types

```typescript
/// <reference types="@citizenfx/client" />

// String type
let playerName: string = "John Doe";
const weaponName: string = "WEAPON_PISTOL";

// Number type
let playerHealth: number = 100;
const maxHealth: number = 200;
let playerId: number = GetPlayerServerId(PlayerId());

// Boolean type
let isAlive: boolean = true;
const hasWeapon: boolean = false;

// Undefined and null
let undefinedValue: undefined = undefined;
let nullValue: null = null;
```

### Array Types

```typescript
/// <reference types="@citizenfx/client" />

// String array
let weapons: string[] = ["WEAPON_PISTOL", "WEAPON_KNIFE", "WEAPON_BAT"];
const vehicleModels: string[] = ["adder", "zentorno", "t20"];

// Number array
let playerCoords: number[] = [0, 0, 0];
const spawnPoints: number[][] = [[100, 200, 30], [300, 400, 50]];

// Boolean array
let playerStates: boolean[] = [true, false, true];

// Mixed array (not recommended, use interfaces instead)
let mixedData: any[] = ["string", 123, true];
```

### Tuple Types (Fixed-length arrays)

```typescript
/// <reference types="@citizenfx/client" />

// 3D coordinates
let playerPosition: [number, number, number] = [100, 200, 30];

// Player data tuple
let playerData: [string, number, boolean] = ["John", 25, true];

// RGB color
let color: [number, number, number] = [255, 0, 0];
```

## FiveM-Specific Types

### Vector Types

```typescript
/// <reference types="@citizenfx/client" />

// FiveM provides Vector3 type
let playerPos: Vector3 = GetEntityCoords(PlayerPedId());

// You can also define your own vector types
interface Vector2 {
    x: number;
    y: number;
}

interface Vector3 {
    x: number;
    y: number;
    z: number;
}

// Using with FiveM natives
function getPlayerPosition(): Vector3 {
    const ped = PlayerPedId();
    const coords = GetEntityCoords(ped);
    return {
        x: coords[0],
        y: coords[1],
        z: coords[2]
    };
}
```

### Entity Types

```typescript
/// <reference types="@citizenfx/client" />

// Entity handles are numbers in FiveM
let playerPed: number = PlayerPedId();
let playerVehicle: number = GetVehiclePedIsIn(playerPed, false);

// Type aliases for clarity
type PedHandle = number;
type VehicleHandle = number;
type ObjectHandle = number;

// Using type aliases
function getPlayerVehicle(player: PedHandle): VehicleHandle | 0 {
    return GetVehiclePedIsIn(player, false);
}
```

## Object Types and Interfaces

### Basic Objects

```typescript
/// <reference types="@citizenfx/client" />

// Object with specific properties
interface Player {
    name: string;
    health: number;
    armor: number;
    position: Vector3;
}

// Using the interface
const player: Player = {
    name: "John Doe",
    health: 100,
    armor: 50,
    position: { x: 0, y: 0, z: 0 }
};
```

### Optional Properties

```typescript
/// <reference types="@citizenfx/client" />

interface Vehicle {
    model: string;
    plate: string;
    fuel?: number;        // Optional property
    owner?: string;       // Optional property
}

const vehicle1: Vehicle = {
    model: "adder",
    plate: "ABC123"
    // fuel and owner are optional, so we can omit them
};

const vehicle2: Vehicle = {
    model: "zentorno",
    plate: "XYZ789",
    fuel: 100,
    owner: "John"
};
```

### Union Types

```typescript
/// <reference types="@citizenfx/client" />

// Union of string literals
type WeaponType = "pistol" | "rifle" | "shotgun" | "melee";

// Union of different types
type PlayerStatus = "online" | "offline" | "connecting" | number;

// Function with union return type
function getPlayerStatus(playerId: number): PlayerStatus {
    // Implementation here
    return "online";
}
```

## Function Types

### Function Signatures

```typescript
/// <reference types="@citizenfx/client" />

// Function type
type HealFunction = (amount: number) => void;

// Function with parameters and return type
function healPlayer(amount: number): void {
    const playerPed = PlayerPedId();
    const currentHealth = GetEntityHealth(playerPed);
    const newHealth = Math.min(currentHealth + amount, 200);
    SetEntityHealth(playerPed, newHealth);
}

// Function that returns a value
function getPlayerHealth(): number {
    return GetEntityHealth(PlayerPedId());
}

// Function with multiple parameters
function spawnVehicle(model: string, position: Vector3, heading: number): number {
    // Implementation here
    return 0;
}
```

### Arrow Functions

```typescript
/// <reference types="@citizenfx/client" />

// Arrow function with type annotations
const healPlayer = (amount: number): void => {
    const playerPed = PlayerPedId();
    SetEntityHealth(playerPed, 200);
};

// Arrow function as event handler
AddEventHandler('playerSpawned', (playerId: number) => {
    console.log(`Player ${playerId} spawned`);
});
```

## Generic Types

### Generic Functions

```typescript
/// <reference types="@citizenfx/client" />

// Generic function
function createArray<T>(length: number, value: T): T[] {
    return Array(length).fill(value);
}

// Using the generic function
const healthArray = createArray(10, 100);        // number[]
const nameArray = createArray(5, "Player");      // string[]
const boolArray = createArray(3, true);          // boolean[]
```

### Generic Interfaces

```typescript
/// <reference types="@citizenfx/client" />

// Generic interface
interface Cache<T> {
    get(key: string): T | undefined;
    set(key: string, value: T): void;
    has(key: string): boolean;
}

// Using generic interface
const playerCache: Cache<Player> = {
    get: (key: string) => undefined,
    set: (key: string, value: Player) => {},
    has: (key: string) => false
};
```

## Advanced Types

### Type Guards

```typescript
/// <reference types="@citizenfx/client" />

// Type guard function
function isPlayer(obj: any): obj is Player {
    return obj && 
           typeof obj.name === 'string' && 
           typeof obj.health === 'number';
}

// Using type guard
function processGameObject(obj: any): void {
    if (isPlayer(obj)) {
        // TypeScript knows obj is Player here
        console.log(`Player ${obj.name} has ${obj.health} health`);
    } else {
        console.log('Not a player object');
    }
}
```

### Type Assertions

```typescript
/// <reference types="@citizenfx/client" />

// Type assertion
function getPlayerData(): any {
    return { name: "John", health: 100, armor: 50 };
}

const data = getPlayerData() as Player;

// Alternative syntax
const data2 = <Player>getPlayerData();
```

### Literal Types

```typescript
/// <reference types="@citizenfx/client" />

// String literal types
type NotificationType = "info" | "success" | "error" | "warning";

function showNotification(message: string, type: NotificationType): void {
    // Implementation here
}

// Number literal types
type VehicleSeat = -1 | 0 | 1 | 2 | 3 | 4;

function putPlayerInVehicle(player: number, vehicle: number, seat: VehicleSeat): void {
    SetPedIntoVehicle(player, vehicle, seat);
}
```

## Enums

```typescript
/// <reference types="@citizenfx/client" />

// Numeric enum
enum PlayerState {
    IDLE = 0,
    WALKING = 1,
    RUNNING = 2,
    DRIVING = 3
}

// String enum
enum VehicleClass {
    COMPACTS = "compacts",
    SEDANS = "sedans",
    SUVS = "suvs",
    COUPES = "coupes",
    MUSCLE = "muscle"
}

// Using enums
function updatePlayerState(state: PlayerState): void {
    console.log(`Player state: ${state}`);
}

updatePlayerState(PlayerState.DRIVING);
```

## Utility Types

```typescript
/// <reference types="@citizenfx/client" />

interface Player {
    name: string;
    health: number;
    armor: number;
    position: Vector3;
}

// Partial - makes all properties optional
type PartialPlayer = Partial<Player>;
// Equivalent to: { name?: string; health?: number; armor?: number; position?: Vector3; }

// Pick - selects specific properties
type PlayerStats = Pick<Player, "health" | "armor">;
// Equivalent to: { health: number; armor: number; }

// Omit - excludes specific properties
type PlayerInfo = Omit<Player, "position">;
// Equivalent to: { name: string; health: number; armor: number; }

// Record - creates object type with specific keys and values
type PlayerPermissions = Record<string, boolean>;
// Equivalent to: { [key: string]: boolean; }
```

## Best Practices

1. **Always use explicit types** for function parameters and return values
2. **Use interfaces** for object shapes
3. **Use type aliases** for complex types
4. **Use union types** for values that can be multiple types
5. **Use generic types** for reusable code
6. **Use type guards** to safely check types
7. **Avoid `any`** when possible - use `unknown` instead
8. **Use const assertions** for immutable data

## Common Patterns

### Configuration Objects

```typescript
/// <reference types="@citizenfx/client" />

interface Config {
    readonly maxPlayers: number;
    readonly defaultHealth: number;
    readonly spawnPoints: readonly Vector3[];
}

const config: Config = {
    maxPlayers: 32,
    defaultHealth: 100,
    spawnPoints: [
        { x: 0, y: 0, z: 0 },
        { x: 100, y: 100, z: 0 }
    ]
} as const;
```

### Event Data Types

```typescript
/// <reference types="@citizenfx/client" />

interface PlayerSpawnEvent {
    playerId: number;
    position: Vector3;
    health: number;
}

interface VehicleEnterEvent {
    vehicle: number;
    seat: number;
    playerId: number;
}

// Using with events
AddEventHandler('playerSpawned', (data: PlayerSpawnEvent) => {
    console.log(`Player ${data.playerId} spawned at`, data.position);
});
```

## Next Steps

- Client-Side Development - Client-side TypeScript examples
- Server-Side Development - Server-side TypeScript examples
- Best Practices - Advanced TypeScript patterns 