# Players

The Players module provides simplified methods for managing and manipulating player data, coordinates, health, and other player-related operations.

## Basic Usage

```lua
-- Get player coordinates
local coords = fivem.players.getCoords()

-- Teleport player
fivem.players.teleport({x = 100.0, y = 200.0, z = 30.0})

-- Get/set health
local health = fivem.players.health()
fivem.players.setHealth(100)
```

## Available Methods

### `players.get(playerId)`
Gets the player ped entity.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, uses the local player.

**Returns:**
- `number` - The player ped entity

**Example:**
```lua
local playerPed = fivem.players.get()
local otherPlayerPed = fivem.players.get(5)
```

### `players.getServerId()`
Gets the current player's server ID.

**Returns:**
- `number` - The player's server ID

**Example:**
```lua
local serverId = fivem.players.getServerId()
print('My server ID:', serverId)
```

### `players.getLocalId()`
Gets the current player's local ID.

**Returns:**
- `number` - The player's local ID

**Example:**
```lua
local localId = fivem.players.getLocalId()
print('My local ID:', localId)
```

### `players.getCoords(playerId)`
Gets the player's coordinates.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, uses the local player.

**Returns:**
- `table` - Coordinates table with x, y, z properties

**Example:**
```lua
local coords = fivem.players.getCoords()
print('Position:', coords.x, coords.y, coords.z)

local otherCoords = fivem.players.getCoords(5)
```

### `players.teleport(playerId, coords)`
Teleports a player to the specified coordinates.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, teleports the local player.
- `coords` (table) - Coordinates table with x, y, z properties

**Example:**
```lua
-- Teleport local player
fivem.players.teleport({x = 100.0, y = 200.0, z = 30.0})

-- Teleport specific player
fivem.players.teleport(5, {x = 150.0, y = 250.0, z = 40.0})
```

### `players.health(playerId)`
Gets the player's health.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, uses the local player.

**Returns:**
- `number` - The player's health (0-200)

**Example:**
```lua
local health = fivem.players.health()
print('My health:', health)

local otherHealth = fivem.players.health(5)
```

### `players.setHealth(playerId, health)`
Sets the player's health.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, sets the local player's health.
- `health` (number) - The health value (0-200)

**Example:**
```lua
-- Set local player health
fivem.players.setHealth(100)

-- Set specific player health
fivem.players.setHealth(5, 150)
```

## Common Player Operations

### Player Spawn Management
```lua
fivem.events.add('playerSpawned', function()
  -- Set player health and armor on spawn
  fivem.players.setHealth(200)
  SetPedArmour(fivem.players.get(), 100)
  
  -- Teleport to spawn location
  fivem.players.teleport({x = -1037.74, y = -2738.04, z = 20.17})
end)
```

### Health Monitoring
```lua
-- Monitor player health
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local health = fivem.players.health()
    
    if health < 50 then
      print('Warning: Low health detected!')
      fivem.ui.show('warning', {message = 'Low health!'})
    end
  end
end)
```

### Player Tracking
```lua
-- Track player movement
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(5000)
    local coords = fivem.players.getCoords()
    print('Current position:', coords.x, coords.y, coords.z)
  end
end)
```

### Multi-Player Operations
```lua
-- Get all players in area
local function getPlayersInRadius(centerCoords, radius)
  local players = {}
  local playerCount = GetNumberOfPlayers()
  
  for i = 0, playerCount - 1 do
    local playerId = GetPlayerFromIndex(i)
    local playerCoords = fivem.players.getCoords(playerId)
    local distance = fivem.utils.distance(centerCoords, playerCoords)
    
    if distance <= radius then
      table.insert(players, playerId)
    end
  end
  
  return players
end

-- Usage
local nearbyPlayers = getPlayersInRadius(fivem.players.getCoords(), 50.0)
print('Players nearby:', #nearbyPlayers)
```

## Coordinate System

The library uses a coordinate system with the following structure:

```lua
local coords = {
  x = 100.0,  -- X coordinate (East/West)
  y = 200.0,  -- Y coordinate (North/South)
  z = 30.0    -- Z coordinate (Height)
}
```

### Common Locations
```lua
-- Common spawn locations
local spawnLocations = {
  {x = -1037.74, y = -2738.04, z = 20.17},  -- Airport
  {x = 425.13, y = -979.55, z = 30.71},     -- Mission Row PD
  {x = 195.17, y = -934.8, z = 30.69},      -- Legion Square
  {x = -266.0, y = -957.0, z = 31.22}       -- Vinewood
}

-- Random spawn location
local randomSpawn = spawnLocations[math.random(#spawnLocations)]
fivem.players.teleport(randomSpawn)
```

## Best Practices

1. **Always check if player exists** before performing operations
2. **Use appropriate health values** (0-200 for health, 0-100 for armor)
3. **Validate coordinates** before teleporting players
4. **Handle errors gracefully** when working with multiple players

```lua
-- Good example
local function safeTeleport(playerId, coords)
  if not coords or not coords.x or not coords.y or not coords.z then
    print('Invalid coordinates provided')
    return false
  end
  
  if playerId and not DoesEntityExist(fivem.players.get(playerId)) then
    print('Player does not exist')
    return false
  end
  
  fivem.players.teleport(playerId, coords)
  return true
end
```

## Migration from Traditional Methods

| Traditional | Library Method |
|-------------|----------------|
| `GetPlayerPed(-1)` | `fivem.players.get()` |
| `GetEntityCoords(GetPlayerPed(-1))` | `fivem.players.getCoords()` |
| `SetEntityCoords(GetPlayerPed(-1), x, y, z)` | `fivem.players.teleport({x, y, z})` |
| `GetEntityHealth(GetPlayerPed(-1))` | `fivem.players.health()` |
| `SetEntityHealth(GetPlayerPed(-1), health)` | `fivem.players.setHealth(health)` |

---

**Next:** [Vehicles](Vehicles.md) | **Previous:** [Events](Events.md) 