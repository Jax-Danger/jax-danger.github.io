# Class System

The Class System provides a simple and intuitive way to create object-oriented code in Lua, making it easier to organize and structure your FiveM scripts.

## Compatibility
✅ **Shared** - Works on both client and server-side

## Basic Usage

```lua
-- Create a class
local Player = fivem.Class.create("Player")

-- Or use global access (no fivem prefix needed)
local Player = Class.create("Player")

-- Add constructor
Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
end)

-- Add methods
Player:method("getName", function(self)
  return self.name
end)

-- Create instance
local player = Player:new("John", 150)

-- Or use global access
local player = Player:new("John", 150)

-- Use methods
print(player:getName()) -- Output: John
```

## Available Methods

### `Class.create(name)`
Creates a new class with the specified name.

**Parameters:**
- `name` (string) - The name of the class

**Returns:**
- `table` - The created class

**Example:**
```lua
local MyClass = fivem.Class.create("MyClass")

-- Or use global access
local MyClass = Class.create("MyClass")
```

### `Class:constructor(func)`
Defines the constructor function for the class.

**Parameters:**
- `func` (function) - The constructor function

**Example:**
```lua
local Player = fivem.Class.create("Player")
Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
  self.maxHealth = 200
end)

-- Or use global access
local Player = Class.create("Player")
Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
  self.maxHealth = 200
end)
```

### `Class:method(name, func)`
Adds a method to the class.

**Parameters:**
- `name` (string) - The name of the method
- `func` (function) - The method function

**Example:**
```lua
local Player = fivem.Class.create("Player")

Player:method("getName", function(self)
  return self.name
end)

Player:method("setHealth", function(self, health)
  self.health = math.min(health, self.maxHealth)
end)

-- Or use global access
local Player = Class.create("Player")

Player:method("getName", function(self)
  return self.name
end)

Player:method("setHealth", function(self, health)
  self.health = math.min(health, self.maxHealth)
end)
```

### `Class:static(name, func)`
Adds a static method to the class.

**Parameters:**
- `name` (string) - The name of the static method
- `func` (function) - The static method function

**Example:**
```lua
local Player = fivem.Class.create("Player")

Player:static("createDefault", function(name)
  return Player:new(name, 100)
end)

-- Or use global access
local Player = Class.create("Player")

Player:static("createDefault", function(name)
  return Player:new(name, 100)
end)

-- Usage
local player = Player.createDefault("John")
```

### `Class:new(...)`
Creates a new instance of the class.

**Parameters:**
- `...` - Arguments to pass to the constructor

**Returns:**
- `table` - The new instance

**Example:**
```lua
local Player = fivem.Class.create("Player")
Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
end)

local player = Player:new("John", 150)

-- Or use global access
local Player = Class.create("Player")
Player:constructor(function(self, name, health)
  self.name = name
  self.health = health or 100
end)

local player = Player:new("John", 150)
```

### `Class:extend(name)`
Creates a new class that extends the current class.

**Parameters:**
- `name` (string) - The name of the new class

**Returns:**
- `table` - The new extended class

**Example:**
```lua
local Animal = fivem.Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)

-- Or use global access
local Animal = Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)
```

### `Class:private(name, func)`
Adds a private method to the class (convention-based).

**Parameters:**
- `name` (string) - The name of the private method
- `func` (function) - The private method function

**Example:**
```lua
local Player = fivem.Class.create("Player")

Player:private("validateHealth", function(self, health)
  return health >= 0 and health <= self.maxHealth
end)

Player:method("setHealth", function(self, health)
  if self:validateHealth(health) then
    self.health = health
  end
end)

-- Or use global access
local Player = Class.create("Player")

Player:private("validateHealth", function(self, health)
  return health >= 0 and health <= self.maxHealth
end)

Player:method("setHealth", function(self, health)
  if self:validateHealth(health) then
    self.health = health
  end
end)
```

### `instance:super(...)`
Calls the parent class constructor or method.

**Parameters:**
- `...` - Arguments to pass to the parent

**Example:**
```lua
local Animal = fivem.Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)

-- Or use global access
local Animal = Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)
```

## Complete Class Examples

### Basic Player Class
```lua
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

### Vehicle Class
```lua
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

## Inheritance Examples

### Animal Hierarchy
```lua
-- Base Animal class
local Animal = fivem.Class.create("Animal")

Animal:constructor(function(self, name, species)
  self.name = name
  self.species = species
  self.health = 100
end)

Animal:method("getName", function(self)
  return self.name
end)

Animal:method("getSpecies", function(self)
  return self.species
end)

Animal:method("makeSound", function(self)
  return "Some animal sound"
end)

-- Or use global access
local Animal = Class.create("Animal")

Animal:constructor(function(self, name, species)
  self.name = name
  self.species = species
  self.health = 100
end)

Animal:method("getName", function(self)
  return self.name
end)

Animal:method("getSpecies", function(self)
  return self.species
end)

Animal:method("makeSound", function(self)
  return "Some animal sound"
end)

-- Dog class extending Animal
local Dog = Animal:extend("Dog")

Dog:constructor(function(self, name, breed)
  self:super(name, "Dog") -- Call parent constructor
  self.breed = breed
end)

Dog:method("makeSound", function(self)
  return "Woof!"
end)

Dog:method("getBreed", function(self)
  return self.breed
end)

-- Cat class extending Animal
local Cat = Animal:extend("Cat")

Cat:constructor(function(self, name, color)
  self:super(name, "Cat") -- Call parent constructor
  self.color = color
end)

Cat:method("makeSound", function(self)
  return "Meow!"
end)

Cat:method("getColor", function(self)
  return self.color
end)

-- Usage
local dog = Dog:new("Buddy", "Golden Retriever")
local cat = Cat:new("Whiskers", "Orange")

print(dog:getName()) -- Output: Buddy
print(dog:getSpecies()) -- Output: Dog
print(dog:getBreed()) -- Output: Golden Retriever
print(dog:makeSound()) -- Output: Woof!

print(cat:getName()) -- Output: Whiskers
print(cat:getSpecies()) -- Output: Cat
print(cat:getColor()) -- Output: Orange
print(cat:makeSound()) -- Output: Meow!
```

### Game Entity Class
```lua
-- Base Entity class
local Entity = fivem.Class.create("Entity")

Entity:constructor(function(self, id, coords)
  self.id = id
  self.coords = coords or {x = 0, y = 0, z = 0}
  self.exists = true
end)

Entity:method("getId", function(self)
  return self.id
end)

Entity:method("getCoords", function(self)
  return self.coords
end)

Entity:method("setCoords", function(self, coords)
  self.coords = coords
end)

Entity:method("exists", function(self)
  return self.exists
end)

Entity:method("destroy", function(self)
  self.exists = false
end)

-- Or use global access
local Entity = Class.create("Entity")

Entity:constructor(function(self, id, coords)
  self.id = id
  self.coords = coords or {x = 0, y = 0, z = 0}
  self.exists = true
end)

Entity:method("getId", function(self)
  return self.id
end)

Entity:method("getCoords", function(self)
  return self.coords
end)

Entity:method("setCoords", function(self, coords)
  self.coords = coords
end)

Entity:method("exists", function(self)
  return self.exists
end)

Entity:method("destroy", function(self)
  self.exists = false
end)

-- Player Entity extending Entity
local PlayerEntity = Entity:extend("PlayerEntity")

PlayerEntity:constructor(function(self, id, name, coords)
  self:super(id, coords) -- Call parent constructor
  self.name = name
  self.health = 100
  self.maxHealth = 200
end)

PlayerEntity:method("getName", function(self)
  return self.name
end)

PlayerEntity:method("getHealth", function(self)
  return self.health
end)

PlayerEntity:method("setHealth", function(self, health)
  self.health = math.min(math.max(health, 0), self.maxHealth)
end)

PlayerEntity:method("isAlive", function(self)
  return self.health > 0
end)

-- Vehicle Entity extending Entity
local VehicleEntity = Entity:extend("VehicleEntity")

VehicleEntity:constructor(function(self, id, model, coords)
  self:super(id, coords) -- Call parent constructor
  self.model = model
  self.health = 1000
  self.maxHealth = 1000
end)

VehicleEntity:method("getModel", function(self)
  return self.model
end)

VehicleEntity:method("getHealth", function(self)
  return self.health
end)

VehicleEntity:method("setHealth", function(self, health)
  self.health = math.min(math.max(health, 0), self.maxHealth)
end)

VehicleEntity:method("isDestroyed", function(self)
  return self.health <= 0
end)

-- Usage
local player = PlayerEntity:new(1, "John", {x = 100, y = 200, z = 30})
local vehicle = VehicleEntity:new(2, "adder", {x = 150, y = 250, z = 40})

print(player:getName()) -- Output: John
print(player:getCoords().x) -- Output: 100
print(vehicle:getModel()) -- Output: adder
print(vehicle:getCoords().x) -- Output: 150
```

## Environment-Aware Classes

### Server-Client Communication Class
```lua
local NetworkManager = fivem.Class.create("NetworkManager")

NetworkManager:constructor(function(self)
  self.isServer = fivem.utils.server()
  self.isClient = fivem.utils.client()
end)

NetworkManager:method("sendToServer", function(self, eventName, data)
  if self.isClient then
    fivem.events.emitServer(eventName, data)
  else
    print("Cannot send to server from server")
  end
end)

NetworkManager:method("sendToClient", function(self, playerId, eventName, data)
  if self.isServer then
    fivem.events.emitClient(eventName, playerId, data)
  else
    print("Cannot send to client from client")
  end
end)

NetworkManager:method("broadcast", function(self, eventName, data)
  if self.isServer then
    fivem.events.emitClient(eventName, -1, data)
  else
    print("Cannot broadcast from client")
  end
end)

-- Or use global access
local NetworkManager = Class.create("NetworkManager")

NetworkManager:constructor(function(self)
  self.isServer = utils.server()
  self.isClient = utils.client()
end)

NetworkManager:method("sendToServer", function(self, eventName, data)
  if self.isClient then
    events.emitServer(eventName, data)
  else
    print("Cannot send to server from server")
  end
end)

NetworkManager:method("sendToClient", function(self, playerId, eventName, data)
  if self.isServer then
    events.emitClient(eventName, playerId, data)
  else
    print("Cannot send to client from client")
  end
end)

NetworkManager:method("broadcast", function(self, eventName, data)
  if self.isServer then
    events.emitClient(eventName, -1, data)
  else
    print("Cannot broadcast from client")
  end
end)

-- Usage
local network = NetworkManager:new()

if network.isClient then
  network:sendToServer("playerAction", {action = "heal"})
else
  network:sendToClient(1, "serverResponse", {status = "success"})
end
```

## Advanced Class Features

### Static Methods and Properties
```lua
local Config = fivem.Class.create("Config")

-- Static properties
Config.defaultSettings = {
  debug = false,
  maxPlayers = 32,
  serverName = "My FiveM Server"
}

Config:static("getDefault", function()
  return Config.defaultSettings
end)

Config:static("getSetting", function(key)
  return Config.defaultSettings[key]
end)

Config:static("setSetting", function(key, value)
  Config.defaultSettings[key] = value
end)

-- Or use global access
local Config = Class.create("Config")

-- Static properties
Config.defaultSettings = {
  debug = false,
  maxPlayers = 32,
  serverName = "My FiveM Server"
}

Config:static("getDefault", function()
  return Config.defaultSettings
end)

Config:static("getSetting", function(key)
  return Config.defaultSettings[key]
end)

Config:static("setSetting", function(key, value)
  Config.defaultSettings[key] = value
end)

-- Usage
local settings = Config.getDefault()
print(settings.serverName) -- Output: My FiveM Server

Config.setSetting("debug", true)
print(Config.getSetting("debug")) -- Output: true
```

### Private Methods (Convention-based)
```lua
local BankAccount = fivem.Class.create("BankAccount")

BankAccount:constructor(function(self, accountNumber, initialBalance)
  self.accountNumber = accountNumber
  self.balance = initialBalance or 0
  self.transactions = {}
end)

-- Private method (convention: prefix with underscore)
BankAccount:private("_validateAmount", function(self, amount)
  return amount > 0
end)

BankAccount:private("_addTransaction", function(self, type, amount, description)
  table.insert(self.transactions, {
    type = type,
    amount = amount,
    description = description,
    timestamp = os.time()
  })
end)

-- Public methods
BankAccount:method("deposit", function(self, amount, description)
  if self:_validateAmount(amount) then
    self.balance = self.balance + amount
    self:_addTransaction("deposit", amount, description or "Deposit")
    return true
  end
  return false
end)

BankAccount:method("withdraw", function(self, amount, description)
  if self:_validateAmount(amount) and self.balance >= amount then
    self.balance = self.balance - amount
    self:_addTransaction("withdrawal", amount, description or "Withdrawal")
    return true
  end
  return false
end)

BankAccount:method("getBalance", function(self)
  return self.balance
end)

BankAccount:method("getTransactions", function(self)
  return self.transactions
end)

-- Or use global access
local BankAccount = Class.create("BankAccount")

BankAccount:constructor(function(self, accountNumber, initialBalance)
  self.accountNumber = accountNumber
  self.balance = initialBalance or 0
  self.transactions = {}
end)

-- Private method (convention: prefix with underscore)
BankAccount:private("_validateAmount", function(self, amount)
  return amount > 0
end)

BankAccount:private("_addTransaction", function(self, type, amount, description)
  table.insert(self.transactions, {
    type = type,
    amount = amount,
    description = description,
    timestamp = os.time()
  })
end)

-- Public methods
BankAccount:method("deposit", function(self, amount, description)
  if self:_validateAmount(amount) then
    self.balance = self.balance + amount
    self:_addTransaction("deposit", amount, description or "Deposit")
    return true
  end
  return false
end)

BankAccount:method("withdraw", function(self, amount, description)
  if self:_validateAmount(amount) and self.balance >= amount then
    self.balance = self.balance - amount
    self:_addTransaction("withdrawal", amount, description or "Withdrawal")
    return true
  end
  return false
end)

BankAccount:method("getBalance", function(self)
  return self.balance
end)

BankAccount:method("getTransactions", function(self)
  return self.transactions
end)

-- Usage
local account = BankAccount:new("12345", 1000)
account:deposit(500, "Salary")
account:withdraw(200, "Shopping")

print(account:getBalance()) -- Output: 1300
print(#account:getTransactions()) -- Output: 2
```

## Best Practices

1. **Use descriptive class names** - Make class names clear and specific
2. **Initialize all properties** - Set default values in the constructor
3. **Use private methods for validation** - Keep validation logic private
4. **Check environment when needed** - Use `fivem.utils.server()` or `fivem.utils.client()`
5. **Validate inputs** - Always validate parameters in methods
6. **Use inheritance appropriately** - Don't over-engineer simple classes

```lua
-- Good example
local Player = fivem.Class.create("Player")

Player:constructor(function(self, name, health)
  -- Validate inputs
  if not name or type(name) ~= 'string' then
    error("Player name must be a string")
  end
  
  if health and (type(health) ~= 'number' or health < 0) then
    error("Player health must be a positive number")
  end
  
  -- Initialize properties
  self.name = name
  self.health = health or 100
  self.maxHealth = 200
  self.armor = 0
  self.maxArmor = 100
  self.createdAt = os.time()
end)

-- Private validation method
Player:private("_validateHealth", function(self, health)
  return type(health) == 'number' and health >= 0 and health <= self.maxHealth
end)

-- Public method with validation
Player:method("setHealth", function(self, health)
  if not self:_validateHealth(health) then
    error("Invalid health value")
  end
  
  self.health = health
end)

-- Or use global access
local Player = Class.create("Player")

Player:constructor(function(self, name, health)
  -- Validate inputs
  if not name or type(name) ~= 'string' then
    error("Player name must be a string")
  end
  
  if health and (type(health) ~= 'number' or health < 0) then
    error("Player health must be a positive number")
  end
  
  -- Initialize properties
  self.name = name
  self.health = health or 100
  self.maxHealth = 200
  self.armor = 0
  self.maxArmor = 100
  self.createdAt = os.time()
end)

-- Private validation method
Player:private("_validateHealth", function(self, health)
  return type(health) == 'number' and health >= 0 and health <= self.maxHealth
end)

-- Public method with validation
Player:method("setHealth", function(self, health)
  if not self:_validateHealth(health) then
    error("Invalid health value")
  end
  
  self.health = health
end)

-- Environment-aware class
local GameManager = fivem.Class.create("GameManager")

GameManager:constructor(function(self)
  self.isServer = fivem.utils.server()
  self.isClient = fivem.utils.client()
  
  if self.isServer then
    self.players = {}
  end
end)

GameManager:method("addPlayer", function(self, playerId, playerData)
  if not self.isServer then
    error("addPlayer can only be called on server")
  end
  
  self.players[playerId] = playerData
end)

-- Or use global access
local GameManager = Class.create("GameManager")

GameManager:constructor(function(self)
  self.isServer = utils.server()
  self.isClient = utils.client()
  
  if self.isServer then
    self.players = {}
  end
end)

GameManager:method("addPlayer", function(self, playerId, playerData)
  if not self.isServer then
    error("addPlayer can only be called on server")
  end
  
  self.players[playerId] = playerData
end)
```

## Migration from Traditional OOP

| Traditional | Library Method |
|-------------|----------------|
| `local Class = {}` | `fivem.Class.create("ClassName")` or `Class.create("ClassName")` |
| `function Class.new(...)` | `Class:constructor(function(self, ...) ... end)` |
| `function Class:method(...)` | `Class:method("methodName", function(self, ...) ... end)` |
| `local instance = Class.new(...)` | `Class:new(...)` |
| `instance:method(...)` | `instance:method(...)` |
| `Class.staticMethod = function(...)` | `Class:static("staticMethod", function(...) ... end)` |

---

**Next:** [Examples](Examples.md) | **Previous:** [Utilities](Utilities.md) 