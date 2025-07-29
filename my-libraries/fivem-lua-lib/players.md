# Players

The Players module provides simplified methods for managing and manipulating player data, coordinates, health, and other player-related operations.

## Compatibility
🔄 **Hybrid** - Works on both client and server-side with appropriate fallbacks

- **Client-side**: Uses local player functions when no playerId is provided
- **Server-side**: Requires playerId parameter and uses server-side functions

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
- `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**
- `number` - The player ped entity

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
- `number` - The player's server ID (client-side only)

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
- `number` - The player's local ID (client-side only)

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
- `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**
- `table` - Coordinates table with x, y, z properties

**Example:**
```lua
-- Client-side: Get local player coords
local coords = fivem.players.pos()
print('Position:', coords.x, coords.y, coords.z)

-- Or use global access
local coords = players.pos()
print('Position:', coords.x, coords.y, coords.z)

-- Server-side: Get specific player coords
local otherCoords = fivem.players.pos(5)

-- Or use global access
local otherCoords = players.pos(5)
```

### `players.tp(playerId, coords)`
Teleports a player to the specified coordinates.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, teleports the local player (client-side only).
- `coords` (table) - Coordinates table with x, y, z properties

**Example:**
```lua
-- Client-side: Teleport local player
fivem.players.tp({x = 100.0, y = 200.0, z = 30.0})

-- Or use global access
players.tp({x = 100.0, y = 200.0, z = 30.0})

-- Server-side: Teleport specific player
fivem.players.tp(5, {x = 150.0, y = 250.0, z = 40.0})

-- Or use global access
players.tp(5, {x = 150.0, y = 250.0, z = 40.0})
```

### `players.hp(playerId)`
Gets the player's health.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**
- `number` - The player's health (0-200)

**Example:**
```lua
-- Client-side: Get local player health
local health = fivem.players.hp()
print('My health:', health)

-- Or use global access
local health = players.hp()
print('My health:', health)

-- Server-side: Get specific player health
local otherHealth = fivem.players.hp(5)

-- Or use global access
local otherHealth = players.hp(5)
```

### `players.setHp(playerId, health)`
Sets the player's health.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, sets the local player's health (client-side only).
- `health` (number) - The health value (0-200)

**Example:**
```lua
-- Client-side: Set local player health
fivem.players.setHp(100)

-- Or use global access
players.setHp(100)

-- Server-side: Set specific player health
fivem.players.setHp(5, 150)

-- Or use global access
players.setHp(5, 150)
```

### `players.name(playerId)`
Gets the player's name.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, gets the local player's name (client-side only).

**Returns:**
- `string` - The player's name

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

## Common Player Operations

### Player Spawn Management
```lua
fivem.events.on('playerSpawned', function()
  -- Set player health and armor on spawn
  fivem.players.setHp(200)
  SetPedArmour(fivem.players.get(), 100)
  
  -- Teleport to spawn location
  fivem.players.tp({x = -1037.74, y = -2738.04, z = 20.17})
end)

-- Or use global access
events.on('playerSpawned', function()
  -- Set player health and armor on spawn
  players.setHp(200)
  SetPedArmour(players.get(), 100)
  
  -- Teleport to spawn location
  players.tp({x = -1037.74, y = -2738.04, z = 20.17})
end)
```

### Health Monitoring
```lua
-- Monitor player health
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local health = fivem.players.hp()
    
    if health < 50 then
      print('Warning: Low health detected!')
    end
  end
end)

-- Or use global access
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local health = players.hp()
    
    if health < 50 then
      print('Warning: Low health detected!')
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
    local coords = fivem.players.pos()
    print('Current position:', coords.x, coords.y, coords.z)
  end
end)

-- Or use global access
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(5000)
    local coords = players.pos()
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
    local playerCoords = fivem.players.pos(playerId)
    local distance = fivem.utils.dist(centerCoords, playerCoords)
    
    if distance <= radius then
      table.insert(players, playerId)
    end
  end
  
  return players
end

-- Or use global access
local function getPlayersInRadius(centerCoords, radius)
  local players = {}
  local playerCount = GetNumberOfPlayers()
  
  for i = 0, playerCount - 1 do
    local playerId = GetPlayerFromIndex(i)
    local playerCoords = players.pos(playerId)
    local distance = utils.dist(centerCoords, playerCoords)
    
    if distance <= radius then
      table.insert(players, playerId)
    end
  end
  
  return players
end

-- Usage
local nearbyPlayers = getPlayersInRadius(fivem.players.pos(), 50.0)
print('Players nearby:', #nearbyPlayers)

-- Or use global access
local nearbyPlayers = getPlayersInRadius(players.pos(), 50.0)
print('Players nearby:', #nearbyPlayers)
```

## Server-Side Player Management

```lua
-- Server-side player connection handling
fivem.events.on('playerConnecting', function(name, setKickReason, deferrals)
  local playerId = source
  print('Player connecting:', name, 'ID:', playerId)
end)

-- Or use global access
events.on('playerConnecting', function(name, setKickReason, deferrals)
  local playerId = source
  print('Player connecting:', name, 'ID:', playerId)
end)

-- Server-side player health management
fivem.events.on('healPlayer', function(targetPlayerId)
  local playerId = source
  if targetPlayerId then
    fivem.players.setHp(targetPlayerId, 200)
    fivem.events.emitClient('healed', targetPlayerId, 'You have been healed!')
  else
    fivem.players.setHp(playerId, 200)
    fivem.events.emitClient('healed', playerId, 'You have been healed!')
  end
end)

-- Or use global access
events.on('healPlayer', function(targetPlayerId)
  local playerId = source
  if targetPlayerId then
    players.setHp(targetPlayerId, 200)
    events.emitClient('healed', targetPlayerId, 'You have been healed!')
  else
    players.setHp(playerId, 200)
    events.emitClient('healed', playerId, 'You have been healed!')
  end
end)

-- Server-side player teleportation
fivem.events.on('teleportPlayer', function(targetPlayerId, coords)
  local playerId = source
  if targetPlayerId then
    fivem.players.tp(targetPlayerId, coords)
    fivem.events.emitClient('teleported', targetPlayerId, 'You have been teleported!')
  else
    fivem.players.tp(playerId, coords)
    fivem.events.emitClient('teleported', playerId, 'You have been teleported!')
  end
end)

-- Or use global access
events.on('teleportPlayer', function(targetPlayerId, coords)
  local playerId = source
  if targetPlayerId then
    players.tp(targetPlayerId, coords)
    events.emitClient('teleported', targetPlayerId, 'You have been teleported!')
  else
    players.tp(playerId, coords)
    events.emitClient('teleported', playerId, 'You have been teleported!')
  end
end)
```

## Client-Side Player Management

```lua
-- Client-side spawn handling
fivem.events.on('playerSpawned', function()
  -- Set up player on spawn
  fivem.players.setHp(200)
  SetPedArmour(fivem.players.get(), 100)
end)

-- Or use global access
events.on('playerSpawned', function()
  -- Set up player on spawn
  players.setHp(200)
  SetPedArmour(players.get(), 100)
end)

-- Client-side health monitoring
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local health = fivem.players.hp()
    local coords = fivem.players.pos()
    
    -- Low health warning
    if health < 50 then
      print('Warning: Low health!')
    end
  end
end)

-- Or use global access
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local health = players.hp()
    local coords = players.pos()
    
    -- Low health warning
    if health < 50 then
      print('Warning: Low health!')
    end
  end
end)
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
fivem.players.tp(randomSpawn)

-- Or use global access
players.tp(randomSpawn)
```

## Best Practices

1. **Always check if player exists** before performing operations
2. **Use appropriate health values** (0-200 for health, 0-100 for armor)
3. **Validate coordinates** before teleporting players
4. **Handle errors gracefully** when working with multiple players
5. **Check environment** when using player functions

```lua
-- Good example
local function safeTeleport(playerId, coords)
  if not coords or not coords.x or not coords.y or not coords.z then
    print('Invalid coordinates provided')
    return false
  end
  
  if fivem.isServer and not playerId then
    print('Server-side teleport requires playerId')
    return false
  end
  
  if playerId and not DoesEntityExist(fivem.players.get(playerId)) then
    print('Player does not exist')
    return false
  end
  
  fivem.players.tp(playerId, coords)
  return true
end

-- Or use global access
local function safeTeleport(playerId, coords)
  if not coords or not coords.x or not coords.y or not coords.z then
    print('Invalid coordinates provided')
    return false
  end
  
  if fivem.isServer and not playerId then
    print('Server-side teleport requires playerId')
    return false
  end
  
  if playerId and not DoesEntityExist(players.get(playerId)) then
    print('Player does not exist')
    return false
  end
  
  players.tp(playerId, coords)
  return true
end

-- Environment-specific usage
if fivem.isClient then
  -- Client-side: Can teleport local player
  fivem.players.tp({x = 100, y = 200, z = 30})
  
  -- Or use global access
  players.tp({x = 100, y = 200, z = 30})
else
  -- Server-side: Must specify player ID
  fivem.players.tp(5, {x = 100, y = 200, z = 30})
  
  -- Or use global access
  players.tp(5, {x = 100, y = 200, z = 30})
end
```

## Migration from Traditional Methods

| Traditional | Library Method |
|-------------|----------------|
| `GetPlayerPed(-1)` | `fivem.players.get()` or `players.get()` |
| `GetEntityCoords(GetPlayerPed(-1))` | `fivem.players.pos()` or `players.pos()` |
| `SetEntityCoords(GetPlayerPed(-1), x, y, z)` | `fivem.players.tp({x, y, z})` or `players.tp({x, y, z})` |
| `GetEntityHealth(GetPlayerPed(-1))` | `fivem.players.hp()` or `players.hp()` |
| `SetEntityHealth(GetPlayerPed(-1), health)` | `fivem.players.setHp(health)` or `players.setHp(health)` |
| `GetPlayerName(playerId)` | `fivem.players.name(playerId)` or `players.name(playerId)` |
