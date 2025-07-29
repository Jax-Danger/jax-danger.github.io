---
description: >-
  This guide covers TypeScript development for FiveM with key differences from
  Lua.
---

# FiveM TypeScript Usage Guide

## ⚠️ Critical Import Requirement

**IMPORTANT**: All scripts MUST be imported in their respective main.ts files to be built.

**Client-side (`src/client/main.ts`):**

```typescript
/// <reference types="@citizenfx/client" />
import './my-script';
```

**Server-side (`src/server/main.ts`):**

```typescript
/// <reference types="@citizenfx/server" />
import './my-script';
```

## Basic Types

### Variables

**Lua:**

```lua
local playerName = "John"
local playerHealth = 100
local isAlive = true
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

let playerName: string = "John";
const playerHealth: number = 100;
const isAlive: boolean = true;
```

### Arrays

**Lua:**

```lua
local weapons = {"WEAPON_PISTOL", "WEAPON_KNIFE"}
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

const weapons: string[] = ["WEAPON_PISTOL", "WEAPON_KNIFE"];
```

### Functions

**Lua:**

```lua
function healPlayer(amount)
    local playerPed = PlayerPedId()
    SetEntityHealth(playerPed, 200)
end
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

function healPlayer(amount: number): void {
    const playerPed: number = PlayerPedId();
    SetEntityHealth(playerPed, 200);
}
```

### Events

**Lua:**

```lua
AddEventHandler('onClientResourceStart', function(resourceName)
    print('Resource started!')
end)
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

// Using 'on' for client/server specific events
on('onClientResourceStart', (resourceName: string) => {
    console.log('Resource started!');
});

// Using 'onNet' for cross-side events
onNet('playerJoined', (playerId: number, playerName: string) => {
    console.log(`${playerName} joined the server`);
});
```

### Tables vs Objects

**Lua:**

```lua
local playerData = {
    name = "John",
    health = 100,
    armor = 50
}
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

interface PlayerData {
    name: string;
    health: number;
    armor: number;
}

const playerData: PlayerData = {
    name: "John",
    health: 100,
    armor: 50
};
```

## FiveM-Specific Types

### Vector Types

```typescript
/// <reference types="@citizenfx/client" />

interface Vector3 {
    x: number;
    y: number;
    z: number;
}

let playerPos: Vector3 = { x: 0, y: 0, z: 0 };
```

### Entity Types

```typescript
/// <reference types="@citizenfx/client" />

type PedHandle = number;
type VehicleHandle = number;

let playerPed: PedHandle = PlayerPedId();
```

## Key Differences

### Type References

**TypeScript requires type references at the top of every file:**

```typescript
/// <reference types="@citizenfx/client" />
```

### Import/Export System

**Lua (no imports needed):**

```lua
-- All files are automatically available
```

**TypeScript (must import):**

```typescript
// In main.ts
import './my-script';
```

### Event System: on vs onNet

**`on`** - For client/server specific events:

```typescript
/// <reference types="@citizenfx/client" />

// Client-side only events
on('onClientResourceStart', (resourceName: string) => {
    console.log('Client resource started');
});

// Server-side only events
on('playerConnecting', (name: string, setKickReason: Function) => {
    console.log('Player connecting:', name);
});
```

**`onNet`** - For cross-side events (can be triggered from either side):

```typescript
/// <reference types="@citizenfx/client" />

// Can be triggered from server to client
onNet('updateHealth', (health: number) => {
    console.log('Health updated:', health);
});

// Can be triggered from client to server
onNet('playerAction', (action: string) => {
    console.log('Player performed action:', action);
});
```

### Commands

**Lua:**

```lua
RegisterCommand('heal', function(source, args)
    SetEntityHealth(PlayerPedId(), 200)
end, false)
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

RegisterCommand('heal', (source: number, args: string[]) => {
    SetEntityHealth(PlayerPedId(), 200);
}, false);
```

### Error Handling

**Lua:**

```lua
local success, result = pcall(function()
    -- risky code here
end)
```

**TypeScript:**

```typescript
/// <reference types="@citizenfx/client" />

try {
    // risky code here
} catch (error) {
    console.error('Error:', error);
}
```

## Best Practices

1. **Always add type references** at the top of files
2. **Import all new files** in main.ts
3. **Use TypeScript interfaces** for data structures
4. **Handle errors** with try-catch blocks
5. **Use const** for values that don't change
6. **Use let** for values that do change
7. **Avoid var** (it's function-scoped, not block-scoped)
