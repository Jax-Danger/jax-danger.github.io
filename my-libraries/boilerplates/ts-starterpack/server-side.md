# Server-Side Development

This guide covers the key differences between Lua and TypeScript for server-side FiveM development.

## ⚠️ Critical Import Requirement

**IMPORTANT**: All server-side scripts MUST be imported in `src/server/main.ts` to be built and included in your resource.

```typescript
/// <reference types="@citizenfx/server" />

// Import all your server-side scripts here
import './my-script';
import './another-script';
// Add new imports here as you create new files
```

## Basic Syntax Differences

### Variables and Types

**Lua:**
```lua
local playerName = "John"
local playerMoney = 1000
local isOnline = true
local playerData = {name = "John", money = 1000}
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

let playerName: string = "John";
const playerMoney: number = 1000;
const isOnline: boolean = true;
const playerData: { name: string; money: number } = { name: "John", money: 1000 };
```

### Functions

**Lua:**
```lua
function giveMoney(playerId, amount)
    local player = GetPlayerName(playerId)
    print("Giving " .. amount .. " to " .. player)
end
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

function giveMoney(playerId: number, amount: number): void {
    const player: string = GetPlayerName(playerId);
    console.log(`Giving ${amount} to ${player}`);
}
```

### Events

**Lua:**
```lua
AddEventHandler('playerConnecting', function(name, setKickReason, deferrals)
    print("Player connecting: " .. name)
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

AddEventHandler('playerConnecting', (name: string, setKickReason: Function, deferrals: any) => {
    console.log(`Player connecting: ${name}`);
});
```

## Key Differences in FiveM Development

### Player Events

**Lua:**
```lua
AddEventHandler('playerJoining', function(oldId)
    local playerId = source
    local playerName = GetPlayerName(playerId)
    print(playerName .. " joined the server")
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

AddEventHandler('playerJoining', (oldId: string) => {
    const playerId: number = source;
    const playerName: string = GetPlayerName(playerId);
    console.log(`${playerName} joined the server`);
});
```

### Triggering Client Events

**Lua:**
```lua
TriggerClientEvent('client:notification', playerId, "Hello from server!")
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

TriggerClientEvent('client:notification', playerId, "Hello from server!");
```

### Commands

**Lua:**
```lua
RegisterCommand('give', function(source, args)
    local targetId = tonumber(args[1])
    local amount = tonumber(args[2])
    -- command logic here
end, true)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

RegisterCommand('give', (source: number, args: string[]) => {
    const targetId: number = parseInt(args[0]);
    const amount: number = parseInt(args[1]);
    // command logic here
}, true);
```

### Database Operations

**Lua:**
```lua
-- Using MySQL-Async
MySQL.Async.execute('INSERT INTO players (name, money) VALUES (?, ?)', {
    playerName, playerMoney
}, function(affectedRows)
    print("Player saved: " .. affectedRows .. " rows affected")
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

// Using async/await with a database wrapper
async function savePlayer(playerName: string, playerMoney: number): Promise<void> {
    try {
        const result = await database.query(
            'INSERT INTO players (name, money) VALUES (?, ?)',
            [playerName, playerMoney]
        );
        console.log(`Player saved: ${result.affectedRows} rows affected`);
    } catch (error) {
        console.error('Failed to save player:', error);
    }
}
```

## Common Patterns

### Player Management

**Lua:**
```lua
local players = {}

AddEventHandler('playerJoining', function(oldId)
    local playerId = source
    players[playerId] = {
        name = GetPlayerName(playerId),
        money = 1000
    }
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

interface Player {
    name: string;
    money: number;
}

const players: Map<number, Player> = new Map();

AddEventHandler('playerJoining', (oldId: string) => {
    const playerId: number = source;
    players.set(playerId, {
        name: GetPlayerName(playerId),
        money: 1000
    });
});
```

### Error Handling

**Lua:**
```lua
local success, result = pcall(function()
    -- risky code here
end)
if not success then
    print("Error: " .. tostring(result))
end
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

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
    print("After 1 second")
end)
```

**TypeScript:**
```typescript
/// <reference types="@citizenfx/server" />

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

## Best Practices

1. **Always add type references** at the top of files
2. **Import all new files** in main.ts
3. **Use TypeScript interfaces** for data structures
4. **Handle errors** with try-catch blocks
5. **Validate all input** from clients
6. **Use async/await** for database operations
7. **Use const** for values that don't change
8. **Use let** for values that do change

## Next Steps

- TypeScript Types - Learn about TypeScript type system
- Event System - Working with FiveM events
- Best Practices - Advanced patterns
