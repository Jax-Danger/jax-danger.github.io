# Utilities

The Utilities module provides helpful functions for common operations like waiting, printing, debugging, distance calculations, and more.

## Compatibility
✅ **Shared** - Works on both client and server-side

## Basic Usage

```lua
-- Wait for 1000ms
fivem.utils.wait(1000)

-- Or use global access (no fivem prefix needed)
utils.wait(1000)

-- Print with template support
fivem.utils.print('Player health: ${1}', playerHealth)

-- Or use global access
utils.print('Player health: ${1}', playerHealth)

-- Calculate distance between two points
local distance = fivem.utils.dist(pos1, pos2)

-- Or use global access
local distance = utils.dist(pos1, pos2)
```

## Available Methods

### `utils.wait(ms)`
Waits for the specified number of milliseconds.

**Parameters:**
- `ms` (number) - Milliseconds to wait

**Example:**
```lua
-- Wait for 1 second
fivem.utils.wait(1000)

-- Or use global access
utils.wait(1000)

-- Wait for 500ms
utils.wait(500)
```

### `utils.print(...)`
Enhanced print function with template support.

**Parameters:**
- `...` - Text and variables to print

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

### `utils.debug(...)`
Debug print function that only outputs when debug mode is enabled.

**Parameters:**
- `...` - Text and variables to print

**Example:**
```lua
-- Debug print (only shows if Config.debug is true)
fivem.utils.debug('Player spawned at:', coords)

-- Or use global access
utils.debug('Player spawned at:', coords)

-- Debug with template
utils.debug('Player ${1} spawned at ${2}', playerName, coords)
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
- `num` (number) - Number to round
- `decimals` (number, optional) - Number of decimal places (default: 0)

**Returns:**
- `number` - Rounded number

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

### `utils.tmpl(template, ...)`
String template function for variable interpolation.

**Parameters:**
- `template` (string) - Template string with ${} placeholders
- `...` - Values to substitute

**Returns:**
- `string` - Processed template string

**Example:**
```lua
-- Basic template
local message = fivem.utils.tmpl('Hello ${1}', 'World')

-- Or use global access
local message = utils.tmpl('Hello ${1}', 'World')

-- Template with multiple variables
local message = utils.tmpl('Player ${1} has ${2} health', playerName, health)

-- Template with variable names
local playerName = 'John'
local health = 100
local message = utils.tmpl('Player ${playerName} has ${health} health')
```

### `utils.server()`
Checks if the code is running on the server-side.

**Returns:**
- `boolean` - True if running on server, false otherwise

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
- `boolean` - True if running on client, false otherwise

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

### Time Management
```lua
-- Wait for specific time
fivem.utils.wait(1000) -- Wait 1 second

-- Or use global access
utils.wait(1000) -- Wait 1 second

-- Wait for multiple seconds
for i = 1, 5 do
  utils.wait(1000)
  print('Second:', i)
end

-- Wait for half a second
utils.wait(500)
```

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

-- Usage
logDebug('Player spawned at:', fivem.players.pos())

-- Or use global access
logDebug('Player spawned at:', players.pos())

-- Conditional debug logging
local function logIfDebug(message, ...)
  if fivem.utils.server() then
    utils.debug('[SERVER]', message, ...)
  else
    utils.debug('[CLIENT]', message, ...)
  end
end
```

### Template System
```lua
-- Create notification templates
local function createNotification(type, message, ...)
  local templates = {
    success = '✅ ${1}',
    error = '❌ ${1}',
    warning = '⚠️ ${1}',
    info = 'ℹ️ ${1}'
  }
  
  local template = templates[type] or '${1}'
  return fivem.utils.tmpl(template, message, ...)
end

-- Or use global access
local function createNotification(type, message, ...)
  local templates = {
    success = '✅ ${1}',
    error = '❌ ${1}',
    warning = '⚠️ ${1}',
    info = 'ℹ️ ${1}'
  }
  
  local template = templates[type] or '${1}'
  return utils.tmpl(template, message, ...)
end

-- Usage
local message = createNotification('success', 'Player saved successfully!')
print(message) -- Output: ✅ Player saved successfully!

-- Complex template
local function createPlayerInfo(playerName, health, coords)
  return utils.tmpl('Player: ${playerName} | Health: ${health} | Position: ${coords.x}, ${coords.y}, ${coords.z}')
end
```

### Environment Detection
```lua
-- Environment-aware function
local function environmentAwareFunction()
  if fivem.utils.server() then
    -- Server-side logic
    print('Running server-side logic')
    fivem.events.emitClient('serverEvent', -1, 'Hello from server')
  else
    -- Client-side logic
    print('Running client-side logic')
    fivem.events.emitServer('clientEvent', 'Hello from client')
  end
end

-- Or use global access
local function environmentAwareFunction()
  if utils.server() then
    -- Server-side logic
    print('Running server-side logic')
    events.emitClient('serverEvent', -1, 'Hello from server')
  else
    -- Client-side logic
    print('Running client-side logic')
    events.emitServer('clientEvent', 'Hello from client')
  end
end

-- Environment-specific utilities
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
```

### Number Formatting
```lua
-- Format numbers for display
local function formatNumber(num, decimals)
  return fivem.utils.round(num, decimals or 2)
end

-- Or use global access
local function formatNumber(num, decimals)
  return utils.round(num, decimals or 2)
end

-- Format distance
local function formatDistance(distance)
  if distance < 1000 then
    return utils.round(distance, 1) .. 'm'
  else
    return utils.round(distance / 1000, 2) .. 'km'
  end
end

-- Usage
local distance = fivem.utils.dist(playerPos, targetPos)
local formattedDistance = formatDistance(distance)
print('Distance to target:', formattedDistance)

-- Or use global access
local distance = utils.dist(playerPos, targetPos)
local formattedDistance = formatDistance(distance)
print('Distance to target:', formattedDistance)
```

## Advanced Utility Functions

### Coordinate Utilities
```lua
-- Get random position within radius
local function getRandomPositionInRadius(center, radius)
  local angle = math.random() * 2 * math.pi
  local distance = math.random() * radius
  
  return {
    x = center.x + math.cos(angle) * distance,
    y = center.y + math.sin(angle) * distance,
    z = center.z
  }
end

-- Check if position is valid
local function isValidPosition(coords)
  return coords and coords.x and coords.y and coords.z
end

-- Get midpoint between two positions
local function getMidpoint(pos1, pos2)
  return {
    x = (pos1.x + pos2.x) / 2,
    y = (pos1.y + pos2.y) / 2,
    z = (pos1.z + pos2.z) / 2
  }
end
```

### Time Utilities
```lua
-- Format time
local function formatTime(seconds)
  local hours = math.floor(seconds / 3600)
  local minutes = math.floor((seconds % 3600) / 60)
  local secs = seconds % 60
  
  if hours > 0 then
    return string.format('%02d:%02d:%02d', hours, minutes, secs)
  else
    return string.format('%02d:%02d', minutes, secs)
  end
end

-- Wait with callback
local function waitWithCallback(ms, callback)
  Citizen.CreateThread(function()
    fivem.utils.wait(ms)
    callback()
  end)
end

-- Or use global access
local function waitWithCallback(ms, callback)
  Citizen.CreateThread(function()
    utils.wait(ms)
    callback()
  end)
end

-- Usage
waitWithCallback(5000, function()
  print('5 seconds have passed!')
end)
```

### Validation Utilities
```lua
-- Validate player ID
local function isValidPlayerId(playerId)
  return playerId and type(playerId) == 'number' and playerId >= 0
end

-- Validate coordinates
local function isValidCoordinates(coords)
  return coords and 
         type(coords.x) == 'number' and 
         type(coords.y) == 'number' and 
         type(coords.z) == 'number'
end

-- Validate vehicle model
local function isValidVehicleModel(model)
  return model and type(model) == 'string' and model ~= ''
end
```

## Best Practices

1. **Use appropriate wait times** - Don't use 0ms waits in loops
2. **Validate inputs** - Always check parameters before using them
3. **Use debug logging** - Enable debug mode for development
4. **Check environment when needed** - Use `utils.server()` and `utils.client()`
5. **Format numbers appropriately** - Use `utils.round()` for display

```lua
-- Good example
local function safeUtilityOperation(operation, ...)
  -- Validate inputs
  if not operation or type(operation) ~= 'function' then
    print('Invalid operation provided')
    return false
  end
  
  -- Execute with error handling
  local success, result = pcall(operation, ...)
  
  if not success then
    fivem.utils.debug('Utility operation failed:', result)
    return false
  end
  
  return result
end

-- Or use global access
local function safeUtilityOperation(operation, ...)
  -- Validate inputs
  if not operation or type(operation) ~= 'function' then
    print('Invalid operation provided')
    return false
  end
  
  -- Execute with error handling
  local success, result = pcall(operation, ...)
  
  if not success then
    utils.debug('Utility operation failed:', result)
    return false
  end
  
  return result
end

-- Environment-aware utility usage
if fivem.utils.server() then
  -- Server-side utilities
  fivem.utils.debug('Server utility operation')
else
  -- Client-side utilities
  fivem.utils.debug('Client utility operation')
end

-- Or use global access
if utils.server() then
  -- Server-side utilities
  utils.debug('Server utility operation')
else
  -- Client-side utilities
  utils.debug('Client utility operation')
end
```

## Migration from Traditional Methods

| Traditional | Library Method |
|-------------|----------------|
| `Wait(ms)` | `fivem.utils.wait(ms)` or `utils.wait(ms)` |
| `print(...)` | `fivem.utils.print(...)` or `utils.print(...)` |
| `math.sqrt(dx*dx + dy*dy + dz*dz)` | `fivem.utils.dist(pos1, pos2)` or `utils.dist(pos1, pos2)` |
| `math.floor(num + 0.5)` | `fivem.utils.round(num)` or `utils.round(num)` |
| `string.format(template, ...)` | `fivem.utils.tmpl(template, ...)` or `utils.tmpl(template, ...)` |
| `IsDuplicityVersion()` | `fivem.utils.server()` or `utils.server()` |
| `not IsDuplicityVersion()` | `fivem.utils.client()` or `utils.client()` |
