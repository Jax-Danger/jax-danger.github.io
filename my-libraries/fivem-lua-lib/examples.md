# Examples

This page contains complete, practical examples of how to use the FiveM Lua Library in real-world scenarios.

## Basic Examples

### Simple Player Management

```lua
-- Basic player operations
local function basicPlayerExample()
  -- Get player information
  local coords = fivem.players.getCoords()
  local health = fivem.players.health()
  local serverId = fivem.players.getServerId()
  
  print('Player position:', coords.x, coords.y, coords.z)
  print('Player health:', health)
  print('Server ID:', serverId)
  
  -- Teleport player
  fivem.players.teleport({x = 100.0, y = 200.0, z = 30.0})
  
  -- Set health
  fivem.players.setHealth(200)
end

-- Run the example
basicPlayerExample()
```

### Event Handling

```lua
-- Event handling examples
local function eventExamples()
  -- Player spawn event
  fivem.events.add('playerSpawned', function()
    print('Player spawned!')
    fivem.players.setHealth(200)
    fivem.players.teleport({x = -1037.74, y = -2738.04, z = 20.17})
  end)
  
  -- Resource start event
  fivem.events.add('onResourceStart', function(resourceName)
    if resourceName == GetCurrentResourceName() then
      print('Resource started:', resourceName)
    end
  end)
  
  -- Custom event
  fivem.events.add('myCustomEvent', function(data)
    print('Custom event received:', data.message)
  end)
  
  -- Trigger custom event
  fivem.events.trigger('myCustomEvent', {message = 'Hello from custom event!'})
end

eventExamples()
```

### Vehicle Management

```lua
-- Vehicle management examples
local function vehicleExamples()
  -- Spawn a vehicle
  local vehicle = fivem.vehicles.spawn('adder', {x = 0.0, y = 0.0, z = 30.0})
  
  if vehicle ~= 0 then
    print('Vehicle spawned successfully')
    
    -- Get vehicle coordinates
    local coords = fivem.vehicles.getCoords(vehicle)
    print('Vehicle position:', coords.x, coords.y, coords.z)
    
    -- Teleport vehicle
    fivem.vehicles.teleport(vehicle, {x = 100.0, y = 100.0, z = 30.0})
    
    -- Delete vehicle after 10 seconds
    Citizen.CreateThread(function()
      fivem.utils.wait(10000)
      fivem.vehicles.delete(vehicle)
      print('Vehicle deleted')
    end)
  end
end

vehicleExamples()
```

## Advanced Examples

### Vehicle Spawner System

```lua
-- Complete vehicle spawner system
local function createVehicleSpawnerSystem()
  local spawnedVehicles = {}
  
  -- Vehicle categories
  local vehicleCategories = {
    sports = {
      'adder', 'zentorno', 't20', 'osiris', 'entityxf', 'fmj', 'reaper', 'xa21'
    },
    suv = {
      'baller', 'cavalcade', 'dubsta', 'granger', 'patrol', 'seminole'
    },
    motorcycle = {
      'akuma', 'bati', 'hakuchou', 'pcj', 'ruffian', 'sanchez'
    }
  }
  
  -- Spawn vehicle function
  local function spawnVehicle(category, coords)
    local vehicles = vehicleCategories[category]
    if not vehicles then
      print('Invalid vehicle category:', category)
      return nil
    end
    
    -- Select random vehicle from category
    local model = vehicles[math.random(#vehicles)]
    
    -- Load model
    local hash = GetHashKey(model)
    RequestModel(hash)
    
    while not HasModelLoaded(hash) do
      fivem.utils.wait(10)
    end
    
    -- Spawn vehicle
    local vehicle = fivem.vehicles.spawn(model, coords)
    
    if vehicle ~= 0 then
      -- Set as mission entity
      SetEntityAsMissionEntity(vehicle, true, true)
      
      -- Add to spawned vehicles list
      table.insert(spawnedVehicles, vehicle)
      
      -- Put player in vehicle
      SetPedIntoVehicle(fivem.players.get(), vehicle, -1)
      
      print('Spawned vehicle:', model)
      return vehicle
    end
    
    SetModelAsNoLongerNeeded(hash)
    return nil
  end
  
  -- Clean up all spawned vehicles
  local function cleanupVehicles()
    for _, vehicle in ipairs(spawnedVehicles) do
      if DoesEntityExist(vehicle) then
        fivem.vehicles.delete(vehicle)
      end
    end
    spawnedVehicles = {}
    print('Cleaned up all spawned vehicles')
  end
  
  -- Spawn vehicle commands
  fivem.events.add('spawnSportsCar', function()
    local coords = fivem.players.getCoords()
    spawnVehicle('sports', coords)
  end)
  
  fivem.events.add('spawnSUV', function()
    local coords = fivem.players.getCoords()
    spawnVehicle('suv', coords)
  end)
  
  fivem.events.add('spawnMotorcycle', function()
    local coords = fivem.players.getCoords()
    spawnVehicle('motorcycle', coords)
  end)
  
  fivem.events.add('cleanupVehicles', function()
    cleanupVehicles()
  end)
  
  -- Clean up on resource stop
  fivem.events.add('onResourceStop', function(resourceName)
    if resourceName == GetCurrentResourceName() then
      cleanupVehicles()
    end
  end)
end

createVehicleSpawnerSystem()
```

## Class System Examples

### Player Management with Classes

```lua
-- Player management using the class system
local function createPlayerClassSystem()
  -- Player class
  local Player = Class.create("Player")
  
  Player:constructor(function(self, playerId)
    self.playerId = playerId or fivem.players.getLocalId()
    self.name = GetPlayerName(self.playerId)
    self.money = 1000
    self.level = 1
    self.experience = 0
    self.lastPosition = fivem.players.getCoords()
  end)
  
  Player:method("teleport", function(self, coords)
    fivem.players.teleport(self.playerId, coords)
    self.lastPosition = coords
  end)
  
  Player:method("getHealth", function(self)
    return fivem.players.health(self.playerId)
  end)
  
  Player:method("setHealth", function(self, health)
    fivem.players.setHealth(self.playerId, health)
  end)
  
  Player:method("addMoney", function(self, amount)
    self.money = self.money + amount
    print(self.name .. " now has $" .. self.money)
  end)
  
  Player:method("addExperience", function(self, exp)
    self.experience = self.experience + exp
    
    -- Level up check
    local expNeeded = self.level * 100
    if self.experience >= expNeeded then
      self.level = self.level + 1
      self.experience = self.experience - expNeeded
      print(self.name .. " leveled up to level " .. self.level .. "!")
    end
  end)
  

  
  -- Police officer class (extends Player)
  local PoliceOfficer = Player:extend("PoliceOfficer")
  
  PoliceOfficer:constructor(function(self, playerId, badgeNumber)
    self:super(playerId)
    self.badgeNumber = badgeNumber
    self.rank = "Officer"
    self.arrests = 0
  end)
  
  PoliceOfficer:method("arrest", function(self, targetPlayer)
    if targetPlayer then
      self.arrests = self.arrests + 1
      self:addExperience(50)
      print(self.name .. " arrested " .. targetPlayer.name .. ". Total arrests: " .. self.arrests)
    end
  end)
  
  PoliceOfficer:method("promote", function(self)
    local ranks = {"Officer", "Detective", "Sergeant", "Lieutenant", "Captain"}
    local currentRankIndex = 1
    
    for i, rank in ipairs(ranks) do
      if rank == self.rank then
        currentRankIndex = i
        break
      end
    end
    
    if currentRankIndex < #ranks then
      self.rank = ranks[currentRankIndex + 1]
      print(self.name .. " promoted to " .. self.rank)
    end
  end)
  
  -- Usage examples
  local player = Player:new()
  player:addMoney(500)
  player:addExperience(75)
  
  local officer = PoliceOfficer:new(nil, "12345")
  officer:arrest(player)
  officer:promote()
  

end

createPlayerClassSystem()
```

### Inventory System with Classes

```lua
-- Complete inventory system using classes
local function createInventoryClassSystem()
  -- Item class
  local Item = Class.create("Item")
  
  Item:constructor(function(self, name, quantity, weight, description)
    self.name = name
    self.quantity = quantity or 1
    self.weight = weight or 0.1
    self.description = description or ""
  end)
  
  Item:method("getTotalWeight", function(self)
    return self.quantity * self.weight
  end)
  
  Item:method("use", function(self, player)
    print("Using " .. self.name)
    -- Override in subclasses
  end)
  
  -- Weapon class (extends Item)
  local Weapon = Item:extend("Weapon")
  
  Weapon:constructor(function(self, name, damage, ammo)
    self:super(name, 1, 2.0, "A weapon")
    self.damage = damage
    self.ammo = ammo or 0
  end)
  
  Weapon:method("use", function(self, player)
    print("Equipping " .. self.name .. " with " .. self.ammo .. " ammo")
    -- Give weapon to player
    GiveWeaponToPed(fivem.players.get(player.playerId), GetHashKey(self.name), self.ammo, false, true)
  end)
  
  -- Consumable class (extends Item)
  local Consumable = Item:extend("Consumable")
  
  Consumable:constructor(function(self, name, healthRestore, weight)
    self:super(name, 1, weight or 0.5, "A consumable item")
    self.healthRestore = healthRestore
  end)
  
  Consumable:method("use", function(self, player)
    local currentHealth = player:getHealth()
    local newHealth = math.min(200, currentHealth + self.healthRestore)
    player:setHealth(newHealth)
    print("Used " .. self.name .. ". Health restored by " .. self.healthRestore)
  end)
  
  -- Inventory class
  local Inventory = Class.create("Inventory")
  
  Inventory:constructor(function(self, maxWeight)
    self.items = {}
    self.maxWeight = maxWeight or 50
  end)
  
  Inventory:method("addItem", function(self, item)
    local currentWeight = self:getCurrentWeight()
    local newWeight = currentWeight + item:getTotalWeight()
    
    if newWeight <= self.maxWeight then
      -- Check if item already exists
      for _, existingItem in ipairs(self.items) do
        if existingItem.name == item.name and existingItem:getClassName() == item:getClassName() then
          existingItem.quantity = existingItem.quantity + item.quantity
          return true
        end
      end
      
      table.insert(self.items, item)
      return true
    end
    
    return false
  end)
  
  Inventory:method("removeItem", function(self, itemName, quantity)
    for i, item in ipairs(self.items) do
      if item.name == itemName then
        if item.quantity <= quantity then
          table.remove(self.items, i)
        else
          item.quantity = item.quantity - quantity
        end
        return true
      end
    end
    return false
  end)
  
  Inventory:method("useItem", function(self, itemName, player)
    for _, item in ipairs(self.items) do
      if item.name == itemName then
        item:use(player)
        if item.quantity <= 1 then
          self:removeItem(itemName, 1)
        else
          item.quantity = item.quantity - 1
        end
        return true
      end
    end
    return false
  end)
  
  Inventory:method("getCurrentWeight", function(self)
    local totalWeight = 0
    for _, item in ipairs(self.items) do
      totalWeight = totalWeight + item:getTotalWeight()
    end
    return totalWeight
  end)
  
  Inventory:method("getItemCount", function(self, itemName)
    for _, item in ipairs(self.items) do
      if item.name == itemName then
        return item.quantity
      end
    end
    return 0
  end)
  
  -- Usage example
  local player = Player:new()
  local inventory = Inventory:new(100)
  
  -- Create items
  local bread = Consumable:new("Bread", 25, 0.5)
  local water = Consumable:new("Water", 15, 1.0)
  local pistol = Weapon:new("WEAPON_PISTOL", 25, 12)
  local medkit = Consumable:new("Medkit", 100, 2.0)
  
  -- Add items to inventory
  inventory:addItem(bread)
  inventory:addItem(water)
  inventory:addItem(pistol)
  inventory:addItem(medkit)
  
  -- Use items
  inventory:useItem("Bread", player)
  inventory:useItem("WEAPON_PISTOL", player)
  
  print("Inventory weight:", inventory:getCurrentWeight())
  print("Bread count:", inventory:getItemCount("Bread"))
end

createInventoryClassSystem()
```

## Utility Examples

### Advanced Utility Functions

```lua
-- Advanced utility examples
local function advancedUtilityExamples()
  -- Distance-based player finder
  local function findPlayersInRadius(centerCoords, radius)
    local players = {}
    local playerCount = GetNumberOfPlayers()
    
    for i = 0, playerCount - 1 do
      local playerId = GetPlayerFromIndex(i)
      local playerCoords = fivem.players.getCoords(playerId)
      local distance = fivem.utils.distance(centerCoords, playerCoords)
      
      if distance <= radius then
        table.insert(players, {
          id = playerId,
          name = GetPlayerName(playerId),
          distance = fivem.utils.round(distance, 1)
        })
      end
    end
    
    return players
  end
  
  -- Format time utility
  local function formatPlayTime(seconds)
    local hours = math.floor(seconds / 3600)
    local minutes = math.floor((seconds % 3600) / 60)
    local secs = seconds % 60
    
    if hours > 0 then
      return fivem.utils.template("${h}h ${m}m ${s}s", hours, minutes, secs)
    elseif minutes > 0 then
      return fivem.utils.template("${m}m ${s}s", minutes, secs)
    else
      return fivem.utils.template("${s}s", secs)
    end
  end
  
  -- Safe coordinate validation
  local function validateAndTeleport(playerId, coords)
    if not coords or not coords.x or not coords.y or not coords.z then
      fivem.utils.debug('Invalid coordinates provided')
      return false
    end
    
    -- Check if coordinates are within reasonable bounds
    if math.abs(coords.x) > 10000 or math.abs(coords.y) > 10000 or math.abs(coords.z) > 1000 then
      fivem.utils.debug('Coordinates out of bounds')
      return false
    end
    
    fivem.players.teleport(playerId, coords)
    return true
  end
  
  -- Usage examples
  local playerCoords = fivem.players.getCoords()
  local nearbyPlayers = findPlayersInRadius(playerCoords, 50.0)
  
  for _, player in ipairs(nearbyPlayers) do
    fivem.utils.print("${name} is ${distance} units away", player.name, player.distance)
  end
  
  local playTime = 3661 -- 1 hour, 1 minute, 1 second
  fivem.utils.print("Play time: ${time}", formatPlayTime(playTime))
  
  validateAndTeleport(nil, {x = 100, y = 200, z = 30})
end

advancedUtilityExamples()
```

## Complete Integration Example

### Full Game System

```lua
-- Complete game system integrating all library features
local function createCompleteGameSystem()
  -- Game state
  local gameState = {
    players = {},
    vehicles = {},
    score = 0,
    gameTime = 0
  }
  
  -- Player class
  local GamePlayer = Class.create("GamePlayer")
  
  GamePlayer:constructor(function(self, playerId)
    self.playerId = playerId
    self.name = GetPlayerName(playerId)
    self.score = 0
    self.kills = 0
    self.deaths = 0
    self.lastPosition = fivem.players.getCoords()
    self.inventory = Inventory:new(50)
  end)
  
  GamePlayer:method("addKill", function(self)
    self.kills = self.kills + 1
    self.score = self.score + 100
    gameState.score = gameState.score + 100
  end)
  
  GamePlayer:method("addDeath", function(self)
    self.deaths = self.deaths + 1
  end)
  
  GamePlayer:method("respawn", function(self)
    self:setHealth(200)
    self:teleport({x = -1037.74, y = -2738.04, z = 20.17})
  end)
  
  -- Vehicle class
  local GameVehicle = Class.create("GameVehicle")
  
  GameVehicle:constructor(function(self, vehicleEntity, ownerId)
    self.entity = vehicleEntity
    self.ownerId = ownerId
    self.health = GetVehicleEngineHealth(vehicleEntity)
    self.fuel = GetVehicleFuelLevel(vehicleEntity)
  end)
  
  GameVehicle:method("repair", function(self)
    SetVehicleEngineHealth(self.entity, 1000)
    SetVehicleFixed(self.entity)
    self.health = 1000
  end)
  
  GameVehicle:method("refuel", function(self)
    SetVehicleFuelLevel(self.entity, 100)
    self.fuel = 100
  end)
  
  -- Game manager
  local GameManager = Class.create("GameManager")
  
  GameManager:constructor(function(self)
    self.players = {}
    self.vehicles = {}
    self.gameTime = 0
  end)
  
  GameManager:method("addPlayer", function(self, playerId)
    local player = GamePlayer:new(playerId)
    self.players[playerId] = player
    gameState.players[playerId] = player
  end)
  
  GameManager:method("removePlayer", function(self, playerId)
    self.players[playerId] = nil
    gameState.players[playerId] = nil
  end)
  
  GameManager:method("spawnVehicle", function(self, model, coords, ownerId)
    local vehicle = fivem.vehicles.spawn(model, coords)
    if vehicle ~= 0 then
      local gameVehicle = GameVehicle:new(vehicle, ownerId)
      self.vehicles[vehicle] = gameVehicle
      gameState.vehicles[vehicle] = gameVehicle
      return gameVehicle
    end
    return nil
  end)
  
  GameManager:method("update", function(self)
    self.gameTime = self.gameTime + 1
    
    -- Update all players
    for playerId, player in pairs(self.players) do
      local currentCoords = fivem.players.getCoords(playerId)
      local distance = fivem.utils.distance(player.lastPosition, currentCoords)
      
      if distance > 100 then
        fivem.utils.debug('Player', player.name, 'moved', fivem.utils.round(distance, 1), 'units')
      end
      
      player.lastPosition = currentCoords
    end
    
    -- Update all vehicles
    for vehicleId, vehicle in pairs(self.vehicles) do
      if DoesEntityExist(vehicleId) then
        vehicle.health = GetVehicleEngineHealth(vehicleId)
        vehicle.fuel = GetVehicleFuelLevel(vehicleId)
      else
        self.vehicles[vehicleId] = nil
        gameState.vehicles[vehicleId] = nil
      end
    end
  end)
  
  -- Create game manager instance
  local gameManager = GameManager:new()
  
  -- Event handlers
  fivem.events.add('playerConnecting', function(name, setKickReason, deferrals)
    local playerId = source
    gameManager:addPlayer(playerId)
  end)
  
  fivem.events.add('playerDropped', function(reason)
    local playerId = source
    gameManager:removePlayer(playerId)
  end)
  
  fivem.events.add('playerKilled', function(killerId, victimId)
    if gameState.players[killerId] then
      gameState.players[killerId]:addKill()
    end
    if gameState.players[victimId] then
      gameState.players[victimId]:addDeath()
    end
  end)
  
  -- Game loop
  Citizen.CreateThread(function()
    while true do
      fivem.utils.wait(1000) -- Update every second
      gameManager:update()
      

    end
  end)
  
  -- Clean up on resource stop
  fivem.events.add('onResourceStop', function(resourceName)
    if resourceName == GetCurrentResourceName() then
      -- Clean up all vehicles
      for vehicleId, _ in pairs(gameManager.vehicles) do
        if DoesEntityExist(vehicleId) then
          fivem.vehicles.delete(vehicleId)
        end
      end
      

    end
  end)
  
  return gameManager
end

-- Initialize the complete game system
local gameSystem = createCompleteGameSystem()
```

## Best Practices Summary

1. **Always use error handling** - Wrap operations in pcall when appropriate
2. **Clean up resources** - Delete vehicles, hide UI, and save data when stopping
3. **Use appropriate wait times** - Don't use very short waits in loops
4. **Validate inputs** - Check coordinates, player IDs, and other parameters
5. **Use the class system** - For complex objects and inheritance
6. **Cache frequently used values** - Avoid recalculating the same data
7. **Use template strings** - For readable and maintainable code
8. **Handle events properly** - Clean up event handlers when needed

***

**Previous:** [Class System](class-system.md) | **Next:** [API Reference](broken-reference)
