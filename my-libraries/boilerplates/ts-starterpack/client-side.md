# Client-Side Development

This guide covers the key differences between Lua and TypeScript for client-side FiveM development.

## ⚠️ Critical Import Requirement

**IMPORTANT**: All client-side scripts MUST be imported in `src/client/main.ts` to be built and included in your resource.

```typescript
/// <reference types="@citizenfx/client" />

// Import all your client-side scripts here
import './my-script';
import './another-script';
// Add new imports here as you create new files
```

## Basic Syntax Differences

### Variables and Types

**Lua:**
```lua
local playerName = "John"
local playerHealth = 100
local isAlive = true
local playerCoords = {x = 0, y = 0, z = 0}
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

let playerName: string = "John";
const playerHealth: number = 100;
const isAlive: boolean = true;
const playerCoords: Vector3 = { x: 0, y: 0, z: 0 };
```

### Functions

**Lua:**
```lua
function healPlayer(amount)
    local playerPed = PlayerPedId()
    local currentHealth = GetEntityHealth(playerPed)
    local newHealth = math.min(currentHealth + amount, 200)
    SetEntityHealth(playerPed, newHealth)
end
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

function healPlayer(amount: number): void {
    const playerPed: number = PlayerPedId();
    const currentHealth: number = GetEntityHealth(playerPed);
    const newHealth: number = Math.min(currentHealth + amount, 200);
    SetEntityHealth(playerPed, newHealth);
}
```

### Events

**Lua:**
```lua
AddEventHandler('onClientResourceStart', function(resourceName)
    if GetCurrentResourceName() ~= resourceName then
        return
    end
    print('Resource started!')
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

AddEventHandler('onClientResourceStart', (resourceName: string) => {
    if (GetCurrentResourceName() !== resourceName) return;
    
    console.log('Resource started!');
});
```

### Tables vs Objects/Interfaces

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

## Key Differences in FiveM Development

### Type References

**TypeScript requires type references at the top of every file:**
```typescript
/// <reference types="@citizenfx/client" />
```

This gives you IntelliSense and type checking for FiveM native functions.

### Import/Export System

**Lua (no imports needed):**
```lua
-- All files are automatically available
-- Just call functions directly
```

**TypeScript (must import):**
```typescript
// In main.ts
import './my-script';

// In my-script.ts
export const myFunction = () => {
    console.log('Hello from my script!');
};
```

### Event Registration

**Lua:**
```lua
RegisterNetEvent('myEvent')
AddEventHandler('myEvent', function(data)
    print('Received:', data)
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

RegisterNetEvent('myEvent');
AddEventHandler('myEvent', (data: any) => {
    console.log('Received:', data);
});
```

### Commands

**Lua:**
```lua
RegisterCommand('heal', function(source, args)
    local playerPed = PlayerPedId()
    SetEntityHealth(playerPed, 200)
end, false)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

RegisterCommand('heal', (source: number, args: string[]) => {
    const playerPed: number = PlayerPedId();
    SetEntityHealth(playerPed, 200);
}, false);
```

### Key Bindings

**Lua:**
```lua
RegisterKeyMapping('heal', 'Heal Player', 'keyboard', 'H')
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

RegisterKeyMapping('heal', 'Heal Player', 'keyboard', 'H');
```

## Common Patterns

### Error Handling

**Lua:**
```lua
local success, result = pcall(function()
    -- risky code here
end)
if not success then
    print('Error:', result)
end
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

### Async Operations

**Lua:**
```lua
-- Lua doesn't have async/await, uses callbacks
Citizen.CreateThread(function()
    Wait(1000)
    print('After 1 second')
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

// Using async/await
async function delayedAction(): Promise<void> {
    await new Promise(resolve => setTimeout(resolve, 1000));
    console.log('After 1 second');
}

// Or using FiveM's Wait
setTimeout(() => {
    console.log('After 1 second');
}, 1000);
```

### Arrays vs Lists

**Lua:**
```lua
local weapons = {"WEAPON_PISTOL", "WEAPON_KNIFE"}
for i, weapon in ipairs(weapons) do
    print(weapon)
end
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/client" />

const weapons: string[] = ["WEAPON_PISTOL", "WEAPON_KNIFE"];
weapons.forEach((weapon: string) => {
    console.log(weapon);
});
```

## Best Practices

1. **Always add type references** at the top of files
2. **Import all new files** in main.ts
3. **Use TypeScript interfaces** for data structures
4. **Handle errors** with try-catch blocks
5. **Use const** for values that don't change
6. **Use let** for values that do change
7. **Avoid var** (it's function-scoped, not block-scoped)

## Next Steps

- Server-Side Development - Server-side TypeScript differences
- Event System - Working with FiveM events
- Best Practices - Advanced patterns 