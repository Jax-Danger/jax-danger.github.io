# API Reference

Complete reference documentation for all methods and functions in the FiveM Lua Library.

## Events Module

### `fivem.events.add(eventName, callback)`

Adds an event handler for the specified event.

**Parameters:**

* `eventName` (string) - The name of the event to listen for
* `callback` (function) - The function to execute when the event is triggered

**Returns:** `void`

**Example:**

```lua
fivem.events.add('playerSpawned', function()
  print('Player spawned!')
end)
```

### `fivem.events.modify(eventName, callback)`

Modifies an existing event handler.

**Parameters:**

* `eventName` (string) - The name of the event to modify
* `callback` (function) - The new function to execute

**Returns:** `void`

### `fivem.events.remove(eventName)`

Removes an event handler.

**Parameters:**

* `eventName` (string) - The name of the event to remove

**Returns:** `void`

### `fivem.events.trigger(eventName, ...)`

Triggers a local event.

**Parameters:**

* `eventName` (string) - The name of the event to trigger
* `...` - Additional arguments to pass to the event handlers

**Returns:** `void`

### `fivem.events.triggerServer(eventName, ...)`

Triggers a server event.

**Parameters:**

* `eventName` (string) - The name of the event to trigger on the server
* `...` - Additional arguments to pass to the server

**Returns:** `void`

### `fivem.events.triggerClient(eventName, target, ...)`

Triggers a client event for a specific player.

**Parameters:**

* `eventName` (string) - The name of the event to trigger
* `target` (number) - The player ID to send the event to
* `...` - Additional arguments to pass to the client

**Returns:** `void`

## Players Module

### `fivem.players.get(playerId)`

Gets the player ped entity.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, uses the local player.

**Returns:** `number` - The player ped entity

### `fivem.players.getServerId()`

Gets the current player's server ID.

**Returns:** `number` - The player's server ID

### `fivem.players.getLocalId()`

Gets the current player's local ID.

**Returns:** `number` - The player's local ID

### `fivem.players.getCoords(playerId)`

Gets the player's coordinates.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, uses the local player.

**Returns:** `table` - Coordinates table with x, y, z properties

### `fivem.players.teleport(playerId, coords)`

Teleports a player to the specified coordinates.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, teleports the local player.
* `coords` (table) - Coordinates table with x, y, z properties

**Returns:** `void`

### `fivem.players.health(playerId)`

Gets the player's health.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, uses the local player.

**Returns:** `number` - The player's health (0-200)

### `fivem.players.setHealth(playerId, health)`

Sets the player's health.

**Parameters:**

* `playerId` (number, optional) - The player ID. If not provided, sets the local player's health.
* `health` (number) - The health value (0-200)

**Returns:** `void`

## Vehicles Module

### `fivem.vehicles.get()`

Gets the current vehicle the player is in.

**Returns:** `number` - The vehicle entity, or 0 if not in a vehicle

### `fivem.vehicles.spawn(model, coords)`

Spawns a vehicle at the specified coordinates.

**Parameters:**

* `model` (string) - The vehicle model name (e.g., 'adder', 'zentorno')
* `coords` (table) - Coordinates table with x, y, z properties

**Returns:** `number` - The spawned vehicle entity

### `fivem.vehicles.delete(vehicle)`

Deletes a vehicle entity.

**Parameters:**

* `vehicle` (number) - The vehicle entity to delete

**Returns:** `void`

### `fivem.vehicles.getCoords(vehicle)`

Gets the vehicle's coordinates.

**Parameters:**

* `vehicle` (number) - The vehicle entity

**Returns:** `table` - Coordinates table with x, y, z properties

### `fivem.vehicles.teleport(vehicle, coords)`

Teleports a vehicle to the specified coordinates.

**Parameters:**

* `vehicle` (number) - The vehicle entity
* `coords` (table) - Coordinates table with x, y, z properties

**Returns:** `void`

## Utilities Module

### `fivem.utils.wait(ms)`

Waits for the specified number of milliseconds.

**Parameters:**

* `ms` (number) - Milliseconds to wait

**Returns:** `void`

### `fivem.utils.print(...)`

Prints with template interpolation support.

**Parameters:**

* `...` - Values to print, supports template strings

**Returns:** `void`

### `fivem.utils.debug(...)`

Debug print (only if Config.debug is true).

**Parameters:**

* `...` - Values to print

**Returns:** `void`

### `fivem.utils.distance(pos1, pos2)`

Calculates the distance between two positions.

**Parameters:**

* `pos1` (table) - First position with x, y, z coordinates
* `pos2` (table) - Second position with x, y, z coordinates

**Returns:** `number` - Distance between the positions

### `fivem.utils.round(num, decimals)`

Rounds a number to the specified number of decimal places.

**Parameters:**

* `num` (number) - Number to round
* `decimals` (number, optional) - Number of decimal places (default: 0)

**Returns:** `number` - Rounded number

### `fivem.utils.template(template, ...)`

String interpolation with ${variable} syntax.

**Parameters:**

* `template` (string) - Template string with ${variable} placeholders
* `...` - Values to substitute for variables

**Returns:** `string` - Interpolated string

## Class System

### `Class.create(className)`

Creates a new class with the specified name.

**Parameters:**

* `className` (string) - The name of the class

**Returns:** `table` - The created class

### `Class:constructor(constructorFunction)`

Defines the constructor function for the class.

**Parameters:**

* `constructorFunction` (function) - The constructor function

**Returns:** `void`

### `Class:method(methodName, methodFunction)`

Adds a method to the class.

**Parameters:**

* `methodName` (string) - The name of the method
* `methodFunction` (function) - The method function

**Returns:** `void`

### `Class:static(methodName, methodFunction)`

Adds a static method to the class.

**Parameters:**

* `methodName` (string) - The name of the static method
* `methodFunction` (function) - The static method function

**Returns:** `void`

### `Class:new(...)`

Creates a new instance of the class.

**Parameters:**

* `...` - Arguments to pass to the constructor

**Returns:** `table` - The new instance

### `Class:extend(childClassName)`

Creates a child class that inherits from the parent class.

**Parameters:**

* `childClassName` (string) - The name of the child class

**Returns:** `table` - The child class

### `instance:isInstanceOf(className)`

Checks if an instance is of a specific class or its subclasses.

**Parameters:**

* `className` (string) - The class name to check

**Returns:** `boolean` - True if the instance is of the specified class

### `instance:getClassName()`

Gets the class name of an instance.

**Returns:** `string` - The class name

## Global Access

All methods are also available through global access without the `fivem.` prefix:

```lua
-- Using namespace
fivem.events.add('playerSpawned', function()
  print('Player spawned!')
end)

-- Using global access
events.add('playerSpawned', function()
  print('Player spawned!')
end)
```

## Data Types

### Coordinates Table

```lua
{
  x = number,  -- X coordinate (East/West)
  y = number,  -- Y coordinate (North/South)
  z = number   -- Z coordinate (Height)
}
```

### Player Data

```lua
{
  id = number,           -- Player ID
  name = string,         -- Player name
  health = number,       -- Player health (0-200)
  coords = table,        -- Player coordinates
  money = number,        -- Player money
  level = number         -- Player level
}
```

### Vehicle Data

```lua
{
  entity = number,       -- Vehicle entity ID
  model = string,        -- Vehicle model name
  coords = table,        -- Vehicle coordinates
  health = number,       -- Vehicle health
  fuel = number          -- Vehicle fuel level
}
```

## Error Handling

### Common Error Scenarios

1.  **Invalid Player ID**

    ```lua
    -- Check if player exists before operations
    if DoesEntityExist(fivem.players.get(playerId)) then
      fivem.players.teleport(playerId, coords)
    end
    ```
2.  **Invalid Coordinates**

    ```lua
    -- Validate coordinates before use
    if coords and coords.x and coords.y and coords.z then
      fivem.players.teleport(coords)
    end
    ```
3.  **Vehicle Not Found**

    ```lua
    -- Check if vehicle exists before operations
    local vehicle = fivem.vehicles.get()
    if vehicle ~= 0 then
      fivem.vehicles.delete(vehicle)
    end
    ```

## Performance Considerations

### Optimization Tips

1.  **Cache Frequently Used Values**

    ```lua
    -- Good: Cache player coords
    local playerCoords = fivem.players.getCoords()
    local distance1 = fivem.utils.distance(playerCoords, target1)
    local distance2 = fivem.utils.distance(playerCoords, target2)

    -- Bad: Get coords multiple times
    local distance1 = fivem.utils.distance(fivem.players.getCoords(), target1)
    local distance2 = fivem.utils.distance(fivem.players.getCoords(), target2)
    ```
2.  **Use Appropriate Wait Times**

    ```lua
    -- Good: Reasonable wait time
    Citizen.CreateThread(function()
      while true do
        fivem.utils.wait(1000) -- 1 second
        -- Update logic
      end
    end)

    -- Bad: Too frequent updates
    Citizen.CreateThread(function()
      while true do
        fivem.utils.wait(1) -- 1 millisecond
        -- Update logic
      end
    end)
    ```

## Migration Guide

### From Traditional FiveM Methods

| Traditional                                          | Library Method                                         |
| ---------------------------------------------------- | ------------------------------------------------------ |
| `AddEventHandler('eventName', callback)`             | `fivem.events.add('eventName', callback)`              |
| `TriggerEvent('eventName', ...)`                     | `fivem.events.trigger('eventName', ...)`               |
| `TriggerServerEvent('eventName', ...)`               | `fivem.events.triggerServer('eventName', ...)`         |
| `TriggerClientEvent('eventName', target, ...)`       | `fivem.events.triggerClient('eventName', target, ...)` |
| `GetPlayerPed(-1)`                                   | `fivem.players.get()`                                  |
| `GetEntityCoords(GetPlayerPed(-1))`                  | `fivem.players.getCoords()`                            |
| `SetEntityCoords(GetPlayerPed(-1), x, y, z)`         | `fivem.players.teleport({x, y, z})`                    |
| `GetEntityHealth(GetPlayerPed(-1))`                  | `fivem.players.health()`                               |
| `SetEntityHealth(GetPlayerPed(-1), health)`          | `fivem.players.setHealth(health)`                      |
| `GetVehiclePedIsIn(GetPlayerPed(-1), false)`         | `fivem.vehicles.get()`                                 |
| `CreateVehicle(hash, x, y, z, heading, true, false)` | `fivem.vehicles.spawn(model, coords)`                  |
| `DeleteEntity(vehicle)`                              | `fivem.vehicles.delete(vehicle)`                       |
| `GetEntityCoords(vehicle)`                           | `fivem.vehicles.getCoords(vehicle)`                    |
| `SetEntityCoords(vehicle, x, y, z)`                  | `fivem.vehicles.teleport(vehicle, coords)`             |

\| `Citizen.Wait(ms)` | `fivem.utils.wait(ms)` | | `print(...)` | `fivem.utils.print(...)` | | `GetDistanceBetweenCoords(x1, y1, z1, x2, y2, z2)` | `fivem.utils.distance(pos1, pos2)` | | `math.floor(num + 0.5)` | `fivem.utils.round(num)` | | String concatenation | `fivem.utils.template(template, ...)` |

### From Traditional OOP

| Traditional                      | Class System                                           |
| -------------------------------- | ------------------------------------------------------ |
| `local Player = {}`              | `local Player = Class.create("Player")`                |
| `function Player.new(...)`       | `Player:constructor(function(self, ...) ... end)`      |
| `function Player:method(...)`    | `Player:method("method", function(self, ...) ... end)` |
| `local player = Player.new(...)` | `local player = Player:new(...)`                       |
| `player:method(...)`             | `player:method(...)`                                   |

## Configuration

### Debug Mode

```lua
-- Enable debug mode in your resource
Config = {
  debug = true  -- Set to false in production
}
```

### Custom Settings

```lua
-- Add custom configuration
Config = {
  debug = false,
  defaultHealth = 200,
  defaultMoney = 1000,
  maxVehicles = 10,

}
```

***

**Previous:** [Examples](examples.md) | **Next:** [Home](../Home.md)
