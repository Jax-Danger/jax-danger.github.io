# Class System

The Class System provides a powerful object-oriented programming framework for FiveM development, featuring inheritance, constructors, static methods, and type checking.

## Basic Class Creation

```lua
local Animal = Class.create("Animal")

-- Add constructor
Animal:constructor(function(self, name, age)
  self.name = name
  self.age = age
end)

-- Add methods
Animal:method("speak", function(self)
  print(self.name .. " makes a sound")
end)

-- Create instance
local dog = Animal:new("Rex", 5)
dog:speak()
```

## Available Methods

### `Class.create(className)`
Creates a new class with the specified name.

**Parameters:**
- `className` (string) - The name of the class

**Returns:**
- `table` - The created class

**Example:**
```lua
local Vehicle = Class.create("Vehicle")
local Player = Class.create("Player")
local Item = Class.create("Item")
```

### `Class:constructor(constructorFunction)`
Defines the constructor function for the class.

**Parameters:**
- `constructorFunction` (function) - The constructor function

**Example:**
```lua
local Player = Class.create("Player")

Player:constructor(function(self, name, money)
  self.name = name
  self.money = money or 1000
  self.level = 1
  self.experience = 0
end)
```

### `Class:method(methodName, methodFunction)`
Adds a method to the class.

**Parameters:**
- `methodName` (string) - The name of the method
- `methodFunction` (function) - The method function

**Example:**
```lua
local Player = Class.create("Player")

Player:method("addMoney", function(self, amount)
  self.money = self.money + amount
  print(self.name .. " now has $" .. self.money)
end)

Player:method("getMoney", function(self)
  return self.money
end)
```

### `Class:static(methodName, methodFunction)`
Adds a static method to the class.

**Parameters:**
- `methodName` (string) - The name of the static method
- `methodFunction` (function) - The static method function

**Example:**
```lua
local Vehicle = Class.create("Vehicle")

Vehicle:static("getVehicleTypes", function()
  return {"car", "truck", "motorcycle", "boat"}
end)

Vehicle:static("isValidModel", function(model)
  local validModels = {"adder", "zentorno", "t20"}
  for _, validModel in ipairs(validModels) do
    if validModel == model then
      return true
    end
  end
  return false
end)
```

### `Class:new(...)`
Creates a new instance of the class.

**Parameters:**
- `...` - Arguments to pass to the constructor

**Returns:**
- `table` - The new instance

**Example:**
```lua
local player = Player:new("John Doe", 5000)
local vehicle = Vehicle:new("adder", {x = 100, y = 200, z = 30})
```

## Inheritance

### `Class:extend(childClassName)`
Creates a child class that inherits from the parent class.

**Parameters:**
- `childClassName` (string) - The name of the child class

**Returns:**
- `table` - The child class

**Example:**
```lua
local Animal = Class.create("Animal")

Animal:constructor(function(self, name, age)
  self.name = name
  self.age = age
end)

Animal:method("speak", function(self)
  print(self.name .. " makes a sound")
end)

-- Create child class
local Dog = Animal:extend("Dog")

-- Override constructor
Dog:constructor(function(self, name, age, breed)
  self:super(name, age) -- Call parent constructor
  self.breed = breed
end)

-- Override method
Dog:method("speak", function(self)
  print(self.name .. " barks!")
end)

-- Add new method
Dog:method("fetch", function(self)
  print(self.name .. " fetches the ball")
end)

-- Create instances
local myDog = Dog:new("Buddy", 3, "Golden Retriever")
myDog:speak() -- Output: Buddy barks!
myDog:fetch() -- Output: Buddy fetches the ball
```

## Type Checking

### `instance:isInstanceOf(className)`
Checks if an instance is of a specific class or its subclasses.

**Parameters:**
- `className` (string) - The class name to check

**Returns:**
- `boolean` - True if the instance is of the specified class

**Example:**
```lua
local myDog = Dog:new("Buddy", 3, "Golden Retriever")
print(myDog:isInstanceOf(Dog)) -- true
print(myDog:isInstanceOf(Animal)) -- true
print(myDog:isInstanceOf("Vehicle")) -- false
```

### `instance:getClassName()`
Gets the class name of an instance.

**Returns:**
- `string` - The class name

**Example:**
```lua
local myDog = Dog:new("Buddy", 3, "Golden Retriever")
print(myDog:getClassName()) -- "Dog"
```

## FiveM Integration Examples

### Player Class
```lua
local Player = Class.create("Player")

Player:constructor(function(self, playerId)
  self.playerId = playerId or fivem.players.getLocalId()
  self.ped = fivem.players.get(self.playerId)
  self.name = GetPlayerName(self.playerId)
end)

Player:method("teleport", function(self, coords)
  fivem.players.teleport(self.playerId, coords)
end)

Player:method("getHealth", function(self)
  return fivem.players.health(self.playerId)
end)

Player:method("setHealth", function(self, health)
  fivem.players.setHealth(self.playerId, health)
end)

Player:method("getCoords", function(self)
  return fivem.players.getCoords(self.playerId)
end)

Player:method("isInVehicle", function(self)
  return fivem.vehicles.get() ~= 0
end)

Player:method("getVehicle", function(self)
  local vehicle = fivem.vehicles.get()
  if vehicle ~= 0 then
    return Vehicle:new(vehicle, self.playerId)
  end
  return nil
end)

-- Usage
local player = Player:new()
player:teleport({x = 100.0, y = 200.0, z = 30.0})
print("Player health:", player:getHealth())
```

### Vehicle Class
```lua
local Vehicle = Class.create("Vehicle")

Vehicle:constructor(function(self, vehicleEntity, ownerId)
  self.entity = vehicleEntity or 0
  self.ownerId = ownerId
  self.model = GetDisplayNameFromVehicleModel(GetEntityModel(self.entity))
end)

Vehicle:method("getCoords", function(self)
  return fivem.vehicles.getCoords(self.entity)
end)

Vehicle:method("teleport", function(self, coords)
  fivem.vehicles.teleport(self.entity, coords)
end)

Vehicle:method("delete", function(self)
  fivem.vehicles.delete(self.entity)
  self.entity = 0
end)

Vehicle:method("getHealth", function(self)
  return GetVehicleEngineHealth(self.entity)
end)

Vehicle:method("setHealth", function(self, health)
  SetVehicleEngineHealth(self.entity, health)
end)

Vehicle:method("getSpeed", function(self)
  return GetEntitySpeed(self.entity) * 3.6 -- Convert to km/h
end)

-- Static methods
Vehicle:static("spawn", function(model, coords, ownerId)
  local vehicle = fivem.vehicles.spawn(model, coords)
  if vehicle ~= 0 then
    return Vehicle:new(vehicle, ownerId)
  end
  return nil
end)

-- Usage
local vehicle = Vehicle:spawn("adder", {x = 0, y = 0, z = 30}, 1)
if vehicle then
  print("Vehicle speed:", vehicle:getSpeed())
  vehicle:delete()
end
```

### Inventory System
```lua
local Item = Class.create("Item")

Item:constructor(function(self, name, quantity, weight)
  self.name = name
  self.quantity = quantity or 1
  self.weight = weight or 0.1
end)

Item:method("getTotalWeight", function(self)
  return self.quantity * self.weight
end)

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
      if existingItem.name == item.name then
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

Inventory:method("getCurrentWeight", function(self)
  local totalWeight = 0
  for _, item in ipairs(self.items) do
    totalWeight = totalWeight + item:getTotalWeight()
  end
  return totalWeight
end)

-- Usage
local inventory = Inventory:new(100)
local bread = Item:new("Bread", 5, 0.5)
local water = Item:new("Water", 3, 1.0)

inventory:addItem(bread)
inventory:addItem(water)
print("Inventory weight:", inventory:getCurrentWeight())
```

## Advanced Class Patterns

### Singleton Pattern
```lua
local GameManager = Class.create("GameManager")

GameManager:constructor(function(self)
  if GameManager.instance then
    return GameManager.instance
  end
  
  self.players = {}
  self.vehicles = {}
  self.score = 0
  
  GameManager.instance = self
end)

GameManager:method("addPlayer", function(self, player)
  table.insert(self.players, player)
end)

GameManager:method("getPlayerCount", function(self)
  return #self.players
end)

-- Static method to get instance
GameManager:static("getInstance", function()
  return GameManager:new()
end)

-- Usage
local gameManager = GameManager:getInstance()
local gameManager2 = GameManager:getInstance()
print(gameManager == gameManager2) -- true (same instance)
```

### Observer Pattern
```lua
local EventEmitter = Class.create("EventEmitter")

EventEmitter:constructor(function(self)
  self.events = {}
end)

EventEmitter:method("on", function(self, event, callback)
  if not self.events[event] then
    self.events[event] = {}
  end
  table.insert(self.events[event], callback)
end)

EventEmitter:method("emit", function(self, event, ...)
  if self.events[event] then
    for _, callback in ipairs(self.events[event]) do
      callback(...)
    end
  end
end)

-- Usage with Player class
local Player = Class.create("Player")
Player:constructor(function(self, playerId)
  EventEmitter.constructor(self)
  self.playerId = playerId
end)

-- Mix in EventEmitter methods
for methodName, method in pairs(EventEmitter) do
  if type(method) == "function" and methodName ~= "new" then
    Player[methodName] = method
  end
end

local player = Player:new(1)
player:on("healthChanged", function(oldHealth, newHealth)
  print("Health changed from", oldHealth, "to", newHealth)
end)

player:emit("healthChanged", 100, 75)
```

### Factory Pattern
```lua
local VehicleFactory = Class.create("VehicleFactory")

VehicleFactory:static("createVehicle", function(type, coords)
  local vehicleTypes = {
    sports = {"adder", "zentorno", "t20"},
    suv = {"baller", "cavalcade", "dubsta"},
    motorcycle = {"akuma", "bati", "hakuchou"}
  }
  
  local models = vehicleTypes[type]
  if not models then
    return nil
  end
  
  local randomModel = models[math.random(#models)]
  local vehicle = fivem.vehicles.spawn(randomModel, coords)
  
  if vehicle ~= 0 then
    return Vehicle:new(vehicle)
  end
  
  return nil
end)

-- Usage
local sportsCar = VehicleFactory.createVehicle("sports", {x = 0, y = 0, z = 30})
local suv = VehicleFactory.createVehicle("suv", {x = 10, y = 0, z = 30})
```

## Best Practices

### Method Organization
```lua
local Player = Class.create("Player")

-- Constructor
Player:constructor(function(self, playerId)
  self.playerId = playerId
  self.name = GetPlayerName(playerId)
  self.money = 1000
end)

-- Public methods
Player:method("getName", function(self)
  return self.name
end)

Player:method("getMoney", function(self)
  return self.money
end)

Player:method("addMoney", function(self, amount)
  self.money = self.money + amount
  self:_onMoneyChanged(amount)
end)

-- Private methods (convention with underscore)
Player:method("_onMoneyChanged", function(self, amount)
  print(self.name .. " money changed by: " .. amount)
end)

-- Static methods
Player:static("getAllPlayers", function()
  local players = {}
  local playerCount = GetNumberOfPlayers()
  
  for i = 0, playerCount - 1 do
    local playerId = GetPlayerFromIndex(i)
    table.insert(players, Player:new(playerId))
  end
  
  return players
end)
```

### Error Handling
```lua
local SafePlayer = Class.create("SafePlayer")

SafePlayer:constructor(function(self, playerId)
  if not playerId or playerId < 0 then
    error("Invalid player ID: " .. tostring(playerId))
  end
  
  self.playerId = playerId
  self.name = GetPlayerName(playerId)
  
  if not self.name then
    error("Could not get player name for ID: " .. playerId)
  end
end)

SafePlayer:method("safeTeleport", function(self, coords)
  if not coords or not coords.x or not coords.y or not coords.z then
    print("Invalid coordinates provided")
    return false
  end
  
  local success, result = pcall(function()
    fivem.players.teleport(self.playerId, coords)
  end)
  
  if not success then
    print("Teleport failed:", result)
    return false
  end
  
  return true
end)
```

### Memory Management
```lua
local ResourceManager = Class.create("ResourceManager")

ResourceManager:constructor(function(self)
  self.resources = {}
end)

ResourceManager:method("addResource", function(self, name, resource)
  self.resources[name] = resource
end)

ResourceManager:method("removeResource", function(self, name)
  if self.resources[name] then
    -- Clean up resource
    if self.resources[name].cleanup then
      self.resources[name]:cleanup()
    end
    self.resources[name] = nil
  end
end)

ResourceManager:method("cleanup", function(self)
  for name, _ in pairs(self.resources) do
    self:removeResource(name)
  end
end)

-- Clean up when resource stops
fivem.events.add('onResourceStop', function(resourceName)
  if resourceName == GetCurrentResourceName() then
    -- Clean up any class instances
  end
end)
```

## Migration from Traditional OOP

| Traditional | Class System |
|-------------|--------------|
| `local Player = {}` | `local Player = Class.create("Player")` |
| `function Player.new(...)` | `Player:constructor(function(self, ...) ... end)` |
| `function Player:method(...)` | `Player:method("method", function(self, ...) ... end)` |
| `local player = Player.new(...)` | `local player = Player:new(...)` |
| `player:method(...)` | `player:method(...)` |

---

**Next:** [Examples](Examples.md) | **Previous:** [Utilities](Utilities.md) 