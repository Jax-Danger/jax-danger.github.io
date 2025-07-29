# Examples

This page provides practical examples demonstrating how to use the FiveM Library in real-world scenarios.

## Basic Event Handling

### Simple Event Listener
```lua
-- Listen for player spawn
fivem.events.on('playerSpawned', function()
  print('Player has spawned!')
end)

-- Or use global access (no fivem prefix needed)
events.on('playerSpawned', function()
  print('Player has spawned!')
end)
```

### Custom Event Communication
```lua
-- Server-side: Listen for client requests
fivem.events.on('playerRequestHeal', function()
  local playerId = source
  fivem.players.setHp(playerId, 200)
  fivem.events.emitClient('healComplete', playerId, 'You have been healed!')
end)

-- Or use global access
events.on('playerRequestHeal', function()
  local playerId = source
  players.setHp(playerId, 200)
  events.emitClient('healComplete', playerId, 'You have been healed!')
end)

-- Client-side: Request healing
fivem.events.emitServer('playerRequestHeal')

-- Or use global access
events.emitServer('playerRequestHeal')

-- Client-side: Listen for server response
fivem.events.on('healComplete', function(message)
  print(message)
end)

-- Or use global access
events.on('healComplete', function(message)
  print(message)
end)
```

## Player Management

### Player Health System
```lua
-- Monitor player health
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    
    local health = fivem.players.hp()
    local maxHealth = 200
    
    if health < 50 then
      print('Warning: Low health detected!')
      fivem.events.emitServer('lowHealthWarning')
    end
    
    if health <= 0 then
      print('Player has died!')
      fivem.events.emitServer('playerDied')
    end
  end
end)

-- Or use global access
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    
    local health = players.hp()
    local maxHealth = 200
    
    if health < 50 then
      print('Warning: Low health detected!')
      events.emitServer('lowHealthWarning')
    end
    
    if health <= 0 then
      print('Player has died!')
      events.emitServer('playerDied')
    end
  end
end)
```

### Player Teleportation System
```lua
-- Teleport to saved locations
local savedLocations = {
  home = {x = 100.0, y = 200.0, z = 30.0},
  work = {x = 500.0, y = 600.0, z = 40.0},
  shop = {x = 300.0, y = 400.0, z = 35.0}
}

-- Function to teleport to location
local function teleportToLocation(locationName)
  local location = savedLocations[locationName]
  if location then
    fivem.players.tp(location)
    print('Teleported to ' .. locationName)
  else
    print('Location not found: ' .. locationName)
  end
end

-- Or use global access
local function teleportToLocation(locationName)
  local location = savedLocations[locationName]
  if location then
    players.tp(location)
    print('Teleported to ' .. locationName)
  else
    print('Location not found: ' .. locationName)
  end
end

-- Event handler for teleport commands
fivem.events.on('teleportCommand', function(locationName)
  teleportToLocation(locationName)
end)

-- Or use global access
events.on('teleportCommand', function(locationName)
  teleportToLocation(locationName)
end)
```

## Vehicle Management

### Vehicle Spawning System
```lua
-- Spawn vehicle and put player in it
local function spawnVehicleForPlayer(model, coords)
  -- Request the model
  local hash = GetHashKey(model)
  RequestModel(hash)
  
  while not HasModelLoaded(hash) do
    Citizen.Wait(0)
  end
  
  -- Spawn vehicle
  local vehicle = fivem.vehicles.spawn(model, coords)
  
  if vehicle ~= 0 then
    -- Put player in vehicle
    SetPedIntoVehicle(fivem.players.get(), vehicle, -1)
    SetModelAsNoLongerNeeded(hash)
    return vehicle
  end
  
  return 0
end

-- Or use global access
local function spawnVehicleForPlayer(model, coords)
  -- Request the model
  local hash = GetHashKey(model)
  RequestModel(hash)
  
  while not HasModelLoaded(hash) do
    Citizen.Wait(0)
  end
  
  -- Spawn vehicle
  local vehicle = vehicles.spawn(model, coords)
  
  if vehicle ~= 0 then
    -- Put player in vehicle
    SetPedIntoVehicle(players.get(), vehicle, -1)
    SetModelAsNoLongerNeeded(hash)
    return vehicle
  end
  
  return 0
end

-- Event handler for vehicle spawning
fivem.events.on('spawnVehicle', function(model)
  local playerCoords = fivem.players.pos()
  local vehicle = spawnVehicleForPlayer(model, playerCoords)
  
  if vehicle ~= 0 then
    print('Vehicle spawned successfully!')
  else
    print('Failed to spawn vehicle')
  end
end)

-- Or use global access
events.on('spawnVehicle', function(model)
  local playerCoords = players.pos()
  local vehicle = spawnVehicleForPlayer(model, playerCoords)
  
  if vehicle ~= 0 then
    print('Vehicle spawned successfully!')
  else
    print('Failed to spawn vehicle')
  end
end)
```

### Vehicle Cleanup System
```lua
-- Track spawned vehicles
local spawnedVehicles = {}

-- Spawn and track vehicle
local function spawnAndTrackVehicle(model, coords)
  local vehicle = fivem.vehicles.spawn(model, coords)
  if vehicle ~= 0 then
    table.insert(spawnedVehicles, vehicle)
    return vehicle
  end
  return 0
end

-- Or use global access
local function spawnAndTrackVehicle(model, coords)
  local vehicle = vehicles.spawn(model, coords)
  if vehicle ~= 0 then
    table.insert(spawnedVehicles, vehicle)
    return vehicle
  end
  return 0
end

-- Clean up all spawned vehicles
local function cleanupVehicles()
  for _, vehicle in ipairs(spawnedVehicles) do
    if DoesEntityExist(vehicle) then
      fivem.vehicles.del(vehicle)
    end
  end
  spawnedVehicles = {}
  print('All vehicles cleaned up')
end

-- Or use global access
local function cleanupVehicles()
  for _, vehicle in ipairs(spawnedVehicles) do
    if DoesEntityExist(vehicle) then
      vehicles.del(vehicle)
    end
  end
  spawnedVehicles = {}
  print('All vehicles cleaned up')
end

-- Event handler for cleanup
fivem.events.on('cleanupVehicles', function()
  cleanupVehicles()
end)

-- Or use global access
events.on('cleanupVehicles', function()
  cleanupVehicles()
end)
```

## Utility Functions

### Distance Calculations
```lua
-- Check if player is near a location
local function isPlayerNearLocation(targetCoords, radius)
  local playerCoords = fivem.players.pos()
  local distance = fivem.utils.dist(playerCoords, targetCoords)
  return distance <= radius
end

-- Or use global access
local function isPlayerNearLocation(targetCoords, radius)
  local playerCoords = players.pos()
  local distance = utils.dist(playerCoords, targetCoords)
  return distance <= radius
end

-- Find nearest player
local function findNearestPlayer(maxDistance)
  local nearestPlayer = nil
  local nearestDistance = maxDistance or 100.0
  local playerCoords = fivem.players.pos()
  
  local playerCount = GetNumberOfPlayers()
  for i = 0, playerCount - 1 do
    local playerId = GetPlayerFromIndex(i)
    if playerId ~= PlayerId() then
      local otherPlayerCoords = fivem.players.pos(playerId)
      local distance = fivem.utils.dist(playerCoords, otherPlayerCoords)
      
      if distance < nearestDistance then
        nearestDistance = distance
        nearestPlayer = playerId
      end
    end
  end
  
  return nearestPlayer, nearestDistance
end

-- Or use global access
local function findNearestPlayer(maxDistance)
  local nearestPlayer = nil
  local nearestDistance = maxDistance or 100.0
  local playerCoords = players.pos()
  
  local playerCount = GetNumberOfPlayers()
  for i = 0, playerCount - 1 do
    local playerId = GetPlayerFromIndex(i)
    if playerId ~= PlayerId() then
      local otherPlayerCoords = players.pos(playerId)
      local distance = utils.dist(playerCoords, otherPlayerCoords)
      
      if distance < nearestDistance then
        nearestDistance = distance
        nearestPlayer = playerId
      end
    end
  end
  
  return nearestPlayer, nearestDistance
end
```

### Debug Logging
```lua
-- Debug logging system
local function logDebug(message, ...)
  fivem.utils.debug('[DEBUG]', message, ...)
end

-- Or use global access
local function logDebug(message, ...)
  utils.debug('[DEBUG]', message, ...)
end

-- Usage in functions
local function processPlayerData(playerId)
  logDebug('Processing player data for ID:', playerId)
  
  local playerCoords = fivem.players.pos(playerId)
  logDebug('Player coordinates:', playerCoords.x, playerCoords.y, playerCoords.z)
  
  local health = fivem.players.hp(playerId)
  logDebug('Player health:', health)
end

-- Or use global access
local function processPlayerData(playerId)
  logDebug('Processing player data for ID:', playerId)
  
  local playerCoords = players.pos(playerId)
  logDebug('Player coordinates:', playerCoords.x, playerCoords.y, playerCoords.z)
  
  local health = players.hp(playerId)
  logDebug('Player health:', health)
end
```

## Class System Examples

### Player Class Implementation
```lua
-- Create a Player class
local Player = fivem.Class.create("Player")

Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
  self.maxHealth = 200
  self.armor = 0
  self.maxArmor = 100
end)

Player:method("getName", function(self)
  return self.name
end)

Player:method("getHealth", function(self)
  return self.health
end)

Player:method("setHealth", function(self, health)
  self.health = math.min(math.max(health, 0), self.maxHealth)
end)

Player:method("heal", function(self, amount)
  self:setHealth(self.health + amount)
end)

Player:method("takeDamage", function(self, amount)
  self:setHealth(self.health - amount)
end)

Player:method("isAlive", function(self)
  return self.health > 0
end)

-- Or use global access
local Player = Class.create("Player")

Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
  self.maxHealth = 200
  self.armor = 0
  self.maxArmor = 100
end)

Player:method("getName", function(self)
  return self.name
end)

Player:method("getHealth", function(self)
  return self.health
end)

Player:method("setHealth", function(self, health)
  self.health = math.min(math.max(health, 0), self.maxHealth)
end)

Player:method("heal", function(self, amount)
  self:setHealth(self.health + amount)
end)

Player:method("takeDamage", function(self, amount)
  self:setHealth(self.health - amount)
end)

Player:method("isAlive", function(self)
  return self.health > 0
end)

-- Usage
local player = Player:new("John", 150)
print(player:getName()) -- Output: John
print(player:getHealth()) -- Output: 150

player:takeDamage(50)
print(player:getHealth()) -- Output: 100

player:heal(25)
print(player:getHealth()) -- Output: 125
```

### Vehicle Class Implementation
```lua
-- Create a Vehicle class
local Vehicle = fivem.Class.create("Vehicle")

Vehicle:constructor(function(self, model, coords)
  self.model = model
  self.coords = coords or {x = 0, y = 0, z = 0}
  self.health = 1000
  self.maxHealth = 1000
  self.fuel = 100
  self.maxFuel = 100
end)

Vehicle:method("getModel", function(self)
  return self.model
end)

Vehicle:method("getCoords", function(self)
  return self.coords
end)

Vehicle:method("setCoords", function(self, coords)
  self.coords = coords
end)

Vehicle:method("getHealth", function(self)
  return self.health
end)

Vehicle:method("setHealth", function(self, health)
  self.health = math.min(math.max(health, 0), self.maxHealth)
end)

Vehicle:method("takeDamage", function(self, amount)
  self:setHealth(self.health - amount)
end)

Vehicle:method("isDestroyed", function(self)
  return self.health <= 0
end)

-- Or use global access
local Vehicle = Class.create("Vehicle")

Vehicle:constructor(function(self, model, coords)
  self.model = model
  self.coords = coords or {x = 0, y = 0, z = 0}
  self.health = 1000
  self.maxHealth = 1000
  self.fuel = 100
  self.maxFuel = 100
end)

Vehicle:method("getModel", function(self)
  return self.model
end)

Vehicle:method("getCoords", function(self)
  return self.coords
end)

Vehicle:method("setCoords", function(self, coords)
  self.coords = coords
end)

Vehicle:method("getHealth", function(self)
  return self.health
end)

Vehicle:method("setHealth", function(self, health)
  self.health = math.min(math.max(health, 0), self.maxHealth)
end)

Vehicle:method("takeDamage", function(self, amount)
  self:setHealth(self.health - amount)
end)

Vehicle:method("isDestroyed", function(self)
  return self.health <= 0
end)

-- Usage
local vehicle = Vehicle:new("adder", {x = 100, y = 200, z = 30})
print(vehicle:getModel()) -- Output: adder
print(vehicle:getHealth()) -- Output: 1000

vehicle:takeDamage(500)
print(vehicle:getHealth()) -- Output: 500
print(vehicle:isDestroyed()) -- Output: false
```

## Server-Client Communication

### Server-Side Player Management
```lua
-- Server-side player connection handling
fivem.events.on('playerConnecting', function(name, setKickReason, deferrals)
  local playerId = source
  print('Player connecting:', name, 'ID:', playerId)
  
  -- Store player data
  local playerData = {
    name = name,
    connectedAt = os.time(),
    health = 200
  }
  
  -- You could store this in a database or global table
  -- playerDatabase[playerId] = playerData
end)

-- Or use global access
events.on('playerConnecting', function(name, setKickReason, deferrals)
  local playerId = source
  print('Player connecting:', name, 'ID:', playerId)
  
  -- Store player data
  local playerData = {
    name = name,
    connectedAt = os.time(),
    health = 200
  }
  
  -- You could store this in a database or global table
  -- playerDatabase[playerId] = playerData
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
```

### Client-Side Event Handling
```lua
-- Client-side spawn handling
fivem.events.on('playerSpawned', function()
  -- Set up player on spawn
  fivem.players.setHp(200)
  SetPedArmour(fivem.players.get(), 100)
  
  -- Teleport to spawn location
  fivem.players.tp({x = -1037.74, y = -2738.04, z = 20.17})
end)

-- Or use global access
events.on('playerSpawned', function()
  -- Set up player on spawn
  players.setHp(200)
  SetPedArmour(players.get(), 100)
  
  -- Teleport to spawn location
  players.tp({x = -1037.74, y = -2738.04, z = 20.17})
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
      fivem.events.emitServer('lowHealthWarning')
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
      events.emitServer('lowHealthWarning')
    end
  end
end)
```

## Advanced Examples

### Multi-Player Interaction System
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

-- Event handler for area effects
fivem.events.on('areaHeal', function(radius)
  local playerCoords = fivem.players.pos()
  local nearbyPlayers = getPlayersInRadius(playerCoords, radius)
  
  for _, playerId in ipairs(nearbyPlayers) do
    fivem.players.setHp(playerId, 200)
  end
  
  print('Healed ' .. #nearbyPlayers .. ' players in area')
end)

-- Or use global access
events.on('areaHeal', function(radius)
  local playerCoords = players.pos()
  local nearbyPlayers = getPlayersInRadius(playerCoords, radius)
  
  for _, playerId in ipairs(nearbyPlayers) do
    players.setHp(playerId, 200)
  end
  
  print('Healed ' .. #nearbyPlayers .. ' players in area')
end)
```

### Environment-Aware System
```lua
-- Environment-aware utility functions
local function getEnvironmentInfo()
  if fivem.utils.server() then
    return {
      type = 'server',
      players = GetPlayers(),
      uptime = GetGameTimer()
    }
  else
    return {
      type = 'client',
      playerId = fivem.players.serverId(),
      coords = fivem.players.pos()
    }
  end
end

-- Or use global access
local function getEnvironmentInfo()
  if utils.server() then
    return {
      type = 'server',
      players = GetPlayers(),
      uptime = GetGameTimer()
    }
  else
    return {
      type = 'client',
      playerId = players.serverId(),
      coords = players.pos()
    }
  end
end

-- Environment-specific event handling
local function setupEnvironmentEvents()
  if fivem.utils.server() then
    -- Server-side events
    fivem.events.on('playerConnecting', function(name, setKickReason, deferrals)
      print('Player connecting:', name)
    end)
    
    fivem.events.on('playerDropped', function(reason)
      local playerId = source
      print('Player disconnected:', playerId, 'Reason:', reason)
    end)
  else
    -- Client-side events
    fivem.events.on('playerSpawned', function()
      print('Player spawned')
    end)
    
    fivem.events.on('playerDied', function()
      print('Player died')
    end)
  end
end

-- Or use global access
local function setupEnvironmentEvents()
  if utils.server() then
    -- Server-side events
    events.on('playerConnecting', function(name, setKickReason, deferrals)
      print('Player connecting:', name)
    end)
    
    events.on('playerDropped', function(reason)
      local playerId = source
      print('Player disconnected:', playerId, 'Reason:', reason)
    end)
  else
    -- Client-side events
    events.on('playerSpawned', function()
      print('Player spawned')
    end)
    
    events.on('playerDied', function()
      print('Player died')
    end)
  end
end

-- Initialize environment-specific systems
setupEnvironmentEvents()
```

## Best Practices

1. **Use appropriate wait times** - Don't use 0ms waits in loops
2. **Validate inputs** - Always check parameters before using them
3. **Handle errors gracefully** - Use try-catch patterns when appropriate
4. **Use debug logging** - Enable debug mode for development
5. **Check environment when needed** - Use `fivem.utils.server()` or `fivem.utils.client()`
6. **Clean up resources** - Always clean up vehicles and other entities

```lua
-- Good example with error handling
local function safePlayerOperation(operation, playerId, ...)
  -- Validate player ID
  if not playerId or type(playerId) ~= 'number' then
    print('Invalid player ID provided')
    return false
  end
  
  -- Check if player exists
  if not DoesEntityExist(fivem.players.get(playerId)) then
    print('Player does not exist')
    return false
  end
  
  -- Execute operation with error handling
  local success, result = pcall(operation, playerId, ...)
  
  if not success then
    fivem.utils.debug('Player operation failed:', result)
    return false
  end
  
  return result
end

-- Or use global access
local function safePlayerOperation(operation, playerId, ...)
  -- Validate player ID
  if not playerId or type(playerId) ~= 'number' then
    print('Invalid player ID provided')
    return false
  end
  
  -- Check if player exists
  if not DoesEntityExist(players.get(playerId)) then
    print('Player does not exist')
    return false
  end
  
  -- Execute operation with error handling
  local success, result = pcall(operation, playerId, ...)
  
  if not success then
    utils.debug('Player operation failed:', result)
    return false
  end
  
  return result
end

-- Usage
local success = safePlayerOperation(function(id)
  fivem.players.setHp(id, 200)
  return true
end, 5)

-- Or use global access
local success = safePlayerOperation(function(id)
  players.setHp(id, 200)
  return true
end, 5)
```
