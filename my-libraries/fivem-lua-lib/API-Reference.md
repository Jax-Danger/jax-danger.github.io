# API Reference

This page provides a comprehensive reference of all available methods and functions in the FiveM Library.

## Quick Reference

### Events
- `events.on(eventName, callback)` - Add event handler
- `events.mod(eventName, callback)` - Modify event handler
- `events.off(eventName)` - Remove event handler
- `events.emit(eventName, ...)` - Trigger local event
- `events.emitServer(eventName, ...)` - Trigger server event
- `events.emitClient(eventName, target, ...)` - Trigger client event

### Players
- `players.get(playerId)` - Get player ped
- `players.serverId()` - Get server ID (client-only)
- `players.localId()` - Get local ID (client-only)
- `players.pos(playerId)` - Get position
- `players.tp(playerId, coords)` - Teleport player
- `players.hp(playerId)` - Get health
- `players.setHp(playerId, health)` - Set health
- `players.name(playerId)` - Get player name

### Vehicles
- `vehicles.get()` - Get current vehicle (client-only)
- `vehicles.spawn(model, coords)` - Spawn vehicle (client-only)
- `vehicles.del(vehicle)` - Delete vehicle (client-only)
- `vehicles.pos(vehicle)` - Get vehicle position (client-only)
- `vehicles.tp(vehicle, coords)` - Teleport vehicle (client-only)

### Utilities
- `utils.wait(ms)` - Wait
- `utils.print(...)` - Print with template support
- `utils.debug(...)` - Debug print
- `utils.dist(pos1, pos2)` - Calculate distance
- `utils.round(num, decimals)` - Round number
- `utils.tmpl(template, ...)` - String template
- `utils.server()` - Check if server
- `utils.client()` - Check if client

### Class System
- `Class.create(name)` - Create class
- `Class:new(...)` - Create instance
- `Class:extend(name)` - Extend class

## Events API

### `events.on(eventName, callback)`
Adds an event handler for the specified event.

**Parameters:**
- `eventName` (string) - The name of the event to listen for
- `callback` (function) - The function to execute when the event is triggered

**Returns:**
- `number` - Event handler ID

**Example:**
```lua
-- Using namespace
fivem.events.on('playerSpawned', function()
  print('Player spawned!')
end)

-- Using global access
events.on('playerSpawned', function()
  print('Player spawned!')
end)
```

### `events.mod(eventName, callback)`
Modifies an existing event handler.

**Parameters:**
- `eventName` (string) - The name of the event to modify
- `callback` (function) - The new function to execute

**Returns:**
- `number` - Event handler ID

**Example:**
```lua
-- Using namespace
fivem.events.mod('playerSpawned', function()
  print('Modified spawn handler!')
end)

-- Using global access
events.mod('playerSpawned', function()
  print('Modified spawn handler!')
end)
```

### `events.off(eventName)`
Removes an event handler.

**Parameters:**
- `eventName` (string) - The name of the event to remove

**Returns:**
- `boolean` - Success status

**Example:**
```lua
-- Using namespace
fivem.events.off('playerSpawned')

-- Using global access
events.off('playerSpawned')
```

### `events.emit(eventName, ...)`
Triggers a local event.

**Parameters:**
- `eventName` (string) - The name of the event to trigger
- `...` - Additional arguments to pass to the event handlers

**Returns:**
- `boolean` - Success status

**Example:**
```lua
-- Using namespace
fivem.events.emit('customEvent', 'Hello', 'World')

-- Using global access
events.emit('customEvent', 'Hello', 'World')
```

### `events.emitServer(eventName, ...)`
Triggers a server event.

**Parameters:**
- `eventName` (string) - The name of the event to trigger on the server
- `...` - Additional arguments to pass to the server

**Returns:**
- `boolean` - Success status

**Example:**
```lua
-- Using namespace
fivem.events.emitServer('savePlayerData', playerData)

-- Using global access
events.emitServer('savePlayerData', playerData)
```

### `events.emitClient(eventName, target, ...)`
Triggers a client event for a specific player.

**Parameters:**
- `eventName` (string) - The name of the event to trigger
- `target` (number) - The player ID to send the event to
- `...` - Additional arguments to pass to the client

**Returns:**
- `boolean` - Success status

**Example:**
```lua
-- Using namespace
fivem.events.emitClient('showNotification', playerId, 'Welcome!')

-- Using global access
events.emitClient('showNotification', playerId, 'Welcome!')
```

## Players API

### `players.get(playerId)`
Gets the player ped entity.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**
- `number` - The player ped entity

**Example:**
```lua
-- Using namespace
local playerPed = fivem.players.get()

-- Using global access
local playerPed = players.get()

-- Server-side: Get specific player
local otherPlayerPed = fivem.players.get(5)

-- Using global access
local otherPlayerPed = players.get(5)
```

### `players.serverId()`
Gets the current player's server ID.

**Returns:**
- `number` - The player's server ID (client-side only)

**Example:**
```lua
-- Using namespace
local serverId = fivem.players.serverId()

-- Using global access
local serverId = players.serverId()
```

### `players.localId()`
Gets the current player's local ID.

**Returns:**
- `number` - The player's local ID (client-side only)

**Example:**
```lua
-- Using namespace
local localId = fivem.players.localId()

-- Using global access
local localId = players.localId()
```

### `players.pos(playerId)`
Gets the player's coordinates.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, uses the local player (client-side only).

**Returns:**
- `table` - Coordinates table with x, y, z properties

**Example:**
```lua
-- Using namespace
local coords = fivem.players.pos()

-- Using global access
local coords = players.pos()

-- Server-side: Get specific player coords
local otherCoords = fivem.players.pos(5)

-- Using global access
local otherCoords = players.pos(5)
```

### `players.tp(playerId, coords)`
Teleports a player to the specified coordinates.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, teleports the local player (client-side only).
- `coords` (table) - Coordinates table with x, y, z properties

**Returns:**
- `boolean` - Success status

**Example:**
```lua
-- Using namespace
fivem.players.tp({x = 100.0, y = 200.0, z = 30.0})

-- Using global access
players.tp({x = 100.0, y = 200.0, z = 30.0})

-- Server-side: Teleport specific player
fivem.players.tp(5, {x = 150.0, y = 250.0, z = 40.0})

-- Using global access
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
-- Using namespace
local health = fivem.players.hp()

-- Using global access
local health = players.hp()

-- Server-side: Get specific player health
local otherHealth = fivem.players.hp(5)

-- Using global access
local otherHealth = players.hp(5)
```

### `players.setHp(playerId, health)`
Sets the player's health.

**Parameters:**
- `playerId` (number, optional) - The player ID. If not provided, sets the local player's health (client-side only).
- `health` (number) - The health value (0-200)

**Returns:**
- `boolean` - Success status

**Example:**
```lua
-- Using namespace
fivem.players.setHp(100)

-- Using global access
players.setHp(100)

-- Server-side: Set specific player health
fivem.players.setHp(5, 150)

-- Using global access
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
-- Using namespace
local name = fivem.players.name()

-- Using global access
local name = players.name()

-- Server-side: Get specific player name
local otherName = fivem.players.name(5)

-- Using global access
local otherName = players.name(5)
```

## Vehicles API

### `vehicles.get()`
Gets the current vehicle the player is in.

**Returns:**
- `number` - The vehicle entity, or 0 if not in a vehicle (client-only)

**Example:**
```lua
-- Using namespace
local vehicle = fivem.vehicles.get()

-- Using global access
local vehicle = vehicles.get()
```

### `vehicles.spawn(model, coords)`
Spawns a vehicle at the specified coordinates.

**Parameters:**
- `model` (string) - The vehicle model name (e.g., 'adder', 'zentorno')
- `coords` (table) - Coordinates table with x, y, z properties

**Returns:**
- `number` - The spawned vehicle entity (client-only)

**Example:**
```lua
-- Using namespace
local vehicle = fivem.vehicles.spawn('adder', {x = 100.0, y = 200.0, z = 30.0})

-- Using global access
local vehicle = vehicles.spawn('adder', {x = 100.0, y = 200.0, z = 30.0})
```

### `vehicles.del(vehicle)`
Deletes a vehicle entity.

**Parameters:**
- `vehicle` (number) - The vehicle entity to delete

**Returns:**
- `boolean` - Success status (client-only)

**Example:**
```lua
-- Using namespace
fivem.vehicles.del(vehicle)

-- Using global access
vehicles.del(vehicle)
```

### `vehicles.pos(vehicle)`
Gets the vehicle's coordinates.

**Parameters:**
- `vehicle` (number) - The vehicle entity

**Returns:**
- `table` - Coordinates table with x, y, z properties (client-only)

**Example:**
```lua
-- Using namespace
local coords = fivem.vehicles.pos(vehicle)

-- Using global access
local coords = vehicles.pos(vehicle)
```

### `vehicles.tp(vehicle, coords)`
Teleports a vehicle to the specified coordinates.

**Parameters:**
- `vehicle` (number) - The vehicle entity
- `coords` (table) - Coordinates table with x, y, z properties

**Returns:**
- `boolean` - Success status (client-only)

**Example:**
```lua
-- Using namespace
fivem.vehicles.tp(vehicle, {x = 500.0, y = 500.0, z = 50.0})

-- Using global access
vehicles.tp(vehicle, {x = 500.0, y = 500.0, z = 50.0})
```

## Utilities API

### `utils.wait(ms)`
Waits for the specified number of milliseconds.

**Parameters:**
- `ms` (number) - Milliseconds to wait

**Returns:**
- `void`

**Example:**
```lua
-- Using namespace
fivem.utils.wait(1000)

-- Using global access
utils.wait(1000)
```

### `utils.print(...)`
Enhanced print function with template support.

**Parameters:**
- `...` - Text and variables to print

**Returns:**
- `void`

**Example:**
```lua
-- Using namespace
fivem.utils.print('Hello World')

-- Using global access
utils.print('Hello World')

-- With template
fivem.utils.print('Player ${1} has ${2} health', playerName, health)

-- Using global access
utils.print('Player ${1} has ${2} health', playerName, health)
```

### `utils.debug(...)`
Debug print function that only outputs when debug mode is enabled.

**Parameters:**
- `...` - Text and variables to print

**Returns:**
- `void`

**Example:**
```lua
-- Using namespace
fivem.utils.debug('Debug message')

-- Using global access
utils.debug('Debug message')
```

### `utils.dist(pos1, pos2)`
Calculates the distance between two positions.

**Parameters:**
- `pos1` (table/vector3) - First position with x, y, z coordinates
- `pos2` (table/vector3) - Second position with x, y, z coordinates

**Returns:**
- `number` - Distance between the two positions

**Example:**
```lua
-- Using namespace
local distance = fivem.utils.dist(pos1, pos2)

-- Using global access
local distance = utils.dist(pos1, pos2)
```

### `utils.round(num, decimals)`
Rounds a number to the specified number of decimal places.

**Parameters:**
- `num` (number) - Number to round
- `decimals` (number, optional) - Number of decimal places (default: 0)

**Returns:**
- `number` - Rounded number

**Example:**
```lua
-- Using namespace
local rounded = fivem.utils.round(3.7)

-- Using global access
local rounded = utils.round(3.7)

-- With decimals
local rounded = fivem.utils.round(3.14159, 2)

-- Using global access
local rounded = utils.round(3.14159, 2)
```

### `utils.tmpl(template, ...)`
String template function for variable interpolation.

**Parameters:**
- `template` (string) - Template string with ${} placeholders
- `...` - Values to substitute

**Returns:**
- `string` - Processed template string

**Example:**
```lua
-- Using namespace
local message = fivem.utils.tmpl('Hello ${1}', 'World')

-- Using global access
local message = utils.tmpl('Hello ${1}', 'World')
```

### `utils.server()`
Checks if the code is running on the server-side.

**Returns:**
- `boolean` - True if running on server, false otherwise

**Example:**
```lua
-- Using namespace
if fivem.utils.server() then
  print('Running on server')
end

-- Using global access
if utils.server() then
  print('Running on server')
end
```

### `utils.client()`
Checks if the code is running on the client-side.

**Returns:**
- `boolean` - True if running on client, false otherwise

**Example:**
```lua
-- Using namespace
if fivem.utils.client() then
  print('Running on client')
end

-- Using global access
if utils.client() then
  print('Running on client')
end
```

## Class System API

### `Class.create(name)`
Creates a new class with the specified name.

**Parameters:**
- `name` (string) - The name of the class

**Returns:**
- `table` - The created class

**Example:**
```lua
-- Using namespace
local MyClass = fivem.Class.create("MyClass")

-- Using global access
local MyClass = Class.create("MyClass")
```

### `Class:constructor(func)`
Defines the constructor function for the class.

**Parameters:**
- `func` (function) - The constructor function

**Returns:**
- `table` - The class

**Example:**
```lua
-- Using namespace
MyClass:constructor(function(self, name)
  self.name = name
end)

-- Using global access
MyClass:constructor(function(self, name)
  self.name = name
end)
```

### `Class:method(name, func)`
Adds a method to the class.

**Parameters:**
- `name` (string) - The name of the method
- `func` (function) - The method function

**Returns:**
- `table` - The class

**Example:**
```lua
-- Using namespace
MyClass:method("getName", function(self)
  return self.name
end)

-- Using global access
MyClass:method("getName", function(self)
  return self.name
end)
```

### `Class:static(name, func)`
Adds a static method to the class.

**Parameters:**
- `name` (string) - The name of the static method
- `func` (function) - The static method function

**Returns:**
- `table` - The class

**Example:**
```lua
-- Using namespace
MyClass:static("createDefault", function(name)
  return MyClass:new(name)
end)

-- Using global access
MyClass:static("createDefault", function(name)
  return MyClass:new(name)
end)
```

### `Class:private(name, func)`
Adds a private method to the class (convention-based).

**Parameters:**
- `name` (string) - The name of the private method
- `func` (function) - The private method function

**Returns:**
- `table` - The class

**Example:**
```lua
-- Using namespace
MyClass:private("_validateName", function(self, name)
  return name and type(name) == 'string'
end)

-- Using global access
MyClass:private("_validateName", function(self, name)
  return name and type(name) == 'string'
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
-- Using namespace
local instance = MyClass:new("John")

-- Using global access
local instance = MyClass:new("John")
```

### `Class:extend(name)`
Creates a new class that extends the current class.

**Parameters:**
- `name` (string) - The name of the new class

**Returns:**
- `table` - The new extended class

**Example:**
```lua
-- Using namespace
local ChildClass = MyClass:extend("ChildClass")

-- Using global access
local ChildClass = MyClass:extend("ChildClass")
```

### `instance:super(...)`
Calls the parent class constructor or method.

**Parameters:**
- `...` - Arguments to pass to the parent

**Returns:**
- `void`

**Example:**
```lua
-- Using namespace
ChildClass:constructor(function(self, name, age)
  self:super(name) -- Call parent constructor
  self.age = age
end)

-- Using global access
ChildClass:constructor(function(self, name, age)
  self:super(name) -- Call parent constructor
  self.age = age
end)
```

## Environment Detection

### `fivem.isServer`
Boolean indicating if code is running on server-side.

### `fivem.isClient`
Boolean indicating if code is running on client-side.

**Example:**
```lua
-- Using namespace
if fivem.isServer then
  print('Running on server')
elseif fivem.isClient then
  print('Running on client')
end

-- Using global access
if utils.server() then
  print('Running on server')
elseif utils.client() then
  print('Running on client')
end
```

## Compatibility Matrix

| Module | Client | Server | Shared |
|--------|--------|--------|--------|
| Events | ✅ | ✅ | ✅ |
| Players | ✅ | ✅ | 🔄 |
| Vehicles | ✅ | ❌ | ❌ |
| Utilities | ✅ | ✅ | ✅ |
| Class System | ✅ | ✅ | ✅ |

**Legend:**
- ✅ Full support
- 🔄 Partial support (with limitations)
- ❌ Not supported

## Error Handling

All library functions include built-in error handling and will return appropriate values when operations fail:

- **Events**: Return `false` if event operation fails
- **Players**: Return `nil` or `0` for invalid operations
- **Vehicles**: Return `0` for failed spawns, `false` for failed operations
- **Utilities**: Return `nil` or `false` for failed operations
- **Class System**: Throw errors for invalid operations

## Performance Notes

- **Events**: Minimal overhead, uses native FiveM functions
- **Players**: Direct native function calls, very fast
- **Vehicles**: Model loading can be slow, use async patterns
- **Utilities**: Optimized calculations, minimal overhead
- **Class System**: Metatable-based, very efficient

## Migration Guide

| Traditional | Library Method |
|-------------|----------------|
| `AddEventHandler` | `events.on` |
| `TriggerEvent` | `events.emit` |
| `TriggerServerEvent` | `events.emitServer` |
| `TriggerClientEvent` | `events.emitClient` |
| `GetPlayerPed(-1)` | `players.get()` |
| `GetEntityCoords(GetPlayerPed(-1))` | `players.pos()` |
| `SetEntityCoords(GetPlayerPed(-1), x, y, z)` | `players.tp({x, y, z})` |
| `GetEntityHealth(GetPlayerPed(-1))` | `players.hp()` |
| `SetEntityHealth(GetPlayerPed(-1), health)` | `players.setHp(health)` |
| `GetVehiclePedIsIn(GetPlayerPed(-1), false)` | `vehicles.get()` |
| `CreateVehicle(hash, x, y, z, heading, true, false)` | `vehicles.spawn(model, coords)` |
| `DeleteEntity(vehicle)` | `vehicles.del(vehicle)` |
| `Wait(ms)` | `utils.wait(ms)` |
| `print(...)` | `utils.print(...)` |
| `math.sqrt(dx*dx + dy*dy + dz*dz)` | `utils.dist(pos1, pos2)` |
| `math.floor(num + 0.5)` | `utils.round(num)` |
| `string.format(template, ...)` | `utils.tmpl(template, ...)` |
| `IsDuplicityVersion()` | `utils.server()` |
| `not IsDuplicityVersion()` | `utils.client()` |

