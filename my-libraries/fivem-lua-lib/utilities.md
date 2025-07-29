# Utilities

The Utilities module provides helpful functions for common operations like waiting, printing, debugging, distance calculations, and more.

## Compatibility

✅ **Shared** - Works on both client and server-side

## Available Methods

### `utils.print(...)`

Enhanced print function with template support.

**Parameters:**

* `...` - Text and variables to print

**Example:**

```lua
-- Basic print
fivem.utils.print('Hello World')

-- Or use global access
utils.print('Hello World')

-- Print with template
fivem.utils.print('Player ${1} has ${2} health', playerName, health)

-- Or use global access
utils.print('Player ${1} has ${2} health', playerName, health)

-- Print with variable interpolation
local playerName = 'John'
local health = 100
utils.print('Player ${playerName} has ${health} health')
```

### `utils.dist(pos1, pos2)`

Calculates the distance between two positions.

**Parameters:**

* `pos1` (table/vector3) - First position with x, y, z coordinates
* `pos2` (table/vector3) - Second position with x, y, z coordinates

**Returns:**

* `number` - Distance between the two positions

**Example:**

```lua
-- Calculate distance between two coordinate tables
local pos1 = {x = 100, y = 200, z = 30}
local pos2 = {x = 150, y = 250, z = 40}
local distance = fivem.utils.dist(pos1, pos2)

-- Or use global access
local distance = utils.dist(pos1, pos2)

-- Calculate distance between vector3 objects
local pos1 = vector3(100, 200, 30)
local pos2 = vector3(150, 250, 40)
local distance = utils.dist(pos1, pos2)

-- Calculate distance from player to target
local playerPos = fivem.players.pos()
local targetPos = {x = 500, y = 500, z = 50}
local distance = utils.dist(playerPos, targetPos)
```

### `utils.round(num, decimals)`

Rounds a number to the specified number of decimal places.

**Parameters:**

* `num` (number) - Number to round
* `decimals` (number, optional) - Number of decimal places (default: 0)

**Returns:**

* `number` - Rounded number

**Example:**

```lua
-- Round to whole number
local rounded = fivem.utils.round(3.7) -- Returns 4

-- Or use global access
local rounded = utils.round(3.7) -- Returns 4

-- Round to 2 decimal places
local rounded = utils.round(3.14159, 2) -- Returns 3.14

-- Round distance for display
local distance = utils.dist(playerPos, targetPos)
local displayDistance = utils.round(distance, 2)
print('Distance:', displayDistance)
```

### `utils.server()`

Checks if the code is running on the server-side.

**Returns:**

* `boolean` - True if running on server, false otherwise

**Example:**

```lua
-- Check if running on server
if fivem.utils.server() then
  print('Running on server')
else
  print('Running on client')
end

-- Or use global access
if utils.server() then
  print('Running on server')
else
  print('Running on client')
end

-- Environment-specific logic
if utils.server() then
  -- Server-side code
  fivem.events.emitClient('serverEvent', playerId, data)
else
  -- Client-side code
  fivem.events.emitServer('clientEvent', data)
end
```

### `utils.client()`

Checks if the code is running on the client-side.

**Returns:**

* `boolean` - True if running on client, false otherwise

**Example:**

```lua
-- Check if running on client
if fivem.utils.client() then
  print('Running on client')
else
  print('Running on server')
end

-- Or use global access
if utils.client() then
  print('Running on client')
else
  print('Running on server')
end

-- Environment-specific logic
if utils.client() then
  -- Client-side code
  local vehicle = fivem.vehicles.get()
else
  -- Server-side code
  print('Cannot get vehicle on server')
end
```

## Common Utility Operations

### Distance Calculations

```lua
-- Calculate distance between player and target
local function getDistanceToTarget(targetCoords)
  local playerPos = fivem.players.pos()
  return fivem.utils.dist(playerPos, targetCoords)
end

-- Or use global access
local function getDistanceToTarget(targetCoords)
  local playerPos = players.pos()
  return utils.dist(playerPos, targetCoords)
end

-- Check if player is within range
local function isPlayerInRange(targetCoords, range)
  local distance = getDistanceToTarget(targetCoords)
  return distance <= range
end

-- Usage
local targetPos = {x = 500, y = 500, z = 50}
if isPlayerInRange(targetPos, 100) then
  print('Player is within 100 units of target')
end
```

## Best Practices

1. **Use appropriate wait times** - Don't use 0ms waits in loops
2. **Validate inputs** - Always check parameters before using them
3. **Use debug logging** - Enable debug mode for development
4. **Check environment when needed** - Use `utils.server()` and `utils.client()`
5. **Format numbers appropriately** - Use `utils.round()` for display

## Migration from Traditional Methods

| Traditional                        | Library Method                                                   |
| ---------------------------------- | ---------------------------------------------------------------- |
| `Wait(ms)`                         | `fivem.utils.wait(ms)` or `utils.wait(ms)`                       |
| `print(...)`                       | `fivem.utils.print(...)` or `utils.print(...)`                   |
| `math.sqrt(dx*dx + dy*dy + dz*dz)` | `fivem.utils.dist(pos1, pos2)` or `utils.dist(pos1, pos2)`       |
| `math.floor(num + 0.5)`            | `fivem.utils.round(num)` or `utils.round(num)`                   |
| `string.format(template, ...)`     | `fivem.utils.tmpl(template, ...)` or `utils.tmpl(template, ...)` |
| `IsDuplicityVersion()`             | `fivem.utils.server()` or `utils.server()`                       |
| `not IsDuplicityVersion()`         | `fivem.utils.client()` or `utils.client()`                       |
