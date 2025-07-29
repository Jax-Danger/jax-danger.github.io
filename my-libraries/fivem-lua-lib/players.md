# Players

The Players module provides simplified methods for managing and manipulating player data, coordinates, health, and other player-related operations.

## Compatibility

🔄 **Hybrid** - Works on both client and server-side with appropriate fallbacks

* **Client-side**: Uses local player functions when no playerId is provided
* **Server-side**: Requires playerId parameter and uses server-side functions

## Basic Usage

```lua
-- Get player coordinates
local coords = fivem.players:pos()

-- Or use global access (no fivem prefix needed)
local coords = players:pos()

-- Teleport player
fivem.players:tp({x = 100.0, y = 200.0, z = 30.0})

-- Or use global access
players:tp({x = 100.0, y = 200.0, z = 30.0})

-- Get/set health
local health = fivem.players:hp()
fivem.players:setHp(100)

-- Or use global access
local health = players:hp()
players:setHp(100)
```

## Available Methods

### `players.get(playerId)`

Gets the player ped entity.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**

* `number` - The player ped entity

**Example:**

```lua
-- Client-side: Get local player
local playerPed = fivem.players:get()

-- Or use global access
local playerPed = players:get()

-- Server-side: Get specific player
local otherPlayerPed = fivem.players:get(5)

-- Or use global access
local otherPlayerPed = players:get(5)
```

### `players.serverId()`

Gets the current player's server ID.

**Returns:**

* `number` - The player's server ID (client-side only)

**Example:**

```lua
-- Client-side only
local serverId = fivem.players:serverId()

-- Or use global access
local serverId = players:serverId()
print('My server ID:', serverId)
```

### `players.localId()`

Gets the current player's local ID.

**Returns:**

* `number` - The player's local ID (client-side only)

**Example:**

```lua
-- Client-side only
local localId = fivem.players:localId()

-- Or use global access
local localId = players:localId()
print('My local ID:', localId)
```

### `players.pos(playerId)`

Gets the player's coordinates.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**

* `table` - Coordinates table with x, y, z properties

**Example:**

```lua
-- Client-side: Get local player coords
local coords = fivem.players.pos()
print('Position:', coords.x, coords.y, coords.z)

-- Or use global access
local coords = players.pos()
print('Position:', coords.x, coords.y, coords.z)
```

### `players.tp(playerId, coords)`

Teleports a player to the specified coordinates.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, teleports the local player (client-side only).
* `coords` (table) - Coordinates table with x, y, z properties

**Example:**

```lua
-- Client-side: Teleport local player
fivem.players.tp({x = 100.0, y = 200.0, z = 30.0})

-- Or use global access
players.tp({x = 100.0, y = 200.0, z = 30.0})
```

### `players.hp(playerId)`

Gets the player's health.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**

* `number` - The player's health (0-200)

**Example:**

```lua
-- Client-side: Get local player health
local health = fivem.players.hp()
print('My health:', health)

-- Or use global access
local health = players.hp()
print('My health:', health)
```

### `players.setHp(playerId, health)`

Sets the player's health.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, sets the local player's health (client-side only).
* `health` (number) - The health value (0-200)

**Example:**

```lua
-- Client-side: Set local player health
fivem.players.setHp(100)

-- Or use global access
players.setHp(100)
```

### `players.name(playerId)`

Gets the player's name.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, gets the local player's name (client-side only).

**Returns:**

* `string` - The player's name

**Example:**

```lua
-- Client-side: Get local player name
local name = fivem.players.name()
print('My name:', name)

-- Or use global access
local name = players.name()
print('My name:', name)

-- Server-side: Get specific player name
local otherName = fivem.players.name(5)

-- Or use global access
local otherName = players.name(5)
```

## Best Practices

1. **Always check if player exists** before performing operations
2. **Use appropriate health values** (0-200 for health, 0-100 for armor)
3. **Validate coordinates** before teleporting players
4. **Handle errors gracefully** when working with multiple players
5. **Check environment** when using player functions

## Migration from Traditional Methods

| Traditional                                  | Library Method                                             |
| -------------------------------------------- | ---------------------------------------------------------- |
| `GetPlayerPed(-1)`                           | `fivem.players.get()` or `players.get()`                   |
| `GetEntityCoords(GetPlayerPed(-1))`          | `fivem.players.pos()` or `players.pos()`                   |
| `SetEntityCoords(GetPlayerPed(-1), x, y, z)` | `fivem.players.tp({x, y, z})` or `players.tp({x, y, z})`   |
| `GetEntityHealth(GetPlayerPed(-1))`          | `fivem.players.hp()` or `players.hp()`                     |
| `SetEntityHealth(GetPlayerPed(-1), health)`  | `fivem.players.setHp(health)` or `players.setHp(health)`   |
| `GetPlayerName(playerId)`                    | `fivem.players.name(playerId)` or `players.name(playerId)` |
