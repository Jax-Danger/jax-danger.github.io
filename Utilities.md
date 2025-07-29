# Utilities

The Utilities module provides helpful functions for common operations like waiting, debugging, distance calculations, number rounding, and string template interpolation.

## Basic Usage

```lua
-- Wait
fivem.utils.wait(1000) -- Wait 1 second

-- Debug print
fivem.utils.debug('Debug message')

-- Calculate distance
local distance = fivem.utils.distance({x=0,y=0,z=0}, {x=10,y=0,z=0})

-- Round number
local rounded = fivem.utils.round(3.14159, 2) -- Returns 3.14

-- Template interpolation
local name = "John"
local age = 25
fivem.utils.print("Hello ${name}, you are ${age} years old!") -- Output: Hello John, you are 25 years old!

-- Template function
local message = fivem.utils.template("Welcome to ${city}!", "Los Santos")
print(message) -- Output: Welcome to Los Santos!
```

## Available Methods

### `utils.wait(ms)`
Waits for the specified number of milliseconds.

**Parameters:**
- `ms` (number) - Milliseconds to wait

**Example:**
```lua
-- Wait 1 second
fivem.utils.wait(1000)

-- Wait 500 milliseconds
fivem.utils.wait(500)

-- Wait 2.5 seconds
fivem.utils.wait(2500)
```

### `utils.print(...)`
Prints with template interpolation support.

**Parameters:**
- `...` - Values to print, supports template strings

**Example:**
```lua
local playerName = "John"
local money = 5000

-- Basic print
fivem.utils.print("Player:", playerName, "Money:", money)

-- Template interpolation
fivem.utils.print("Player ${name} has ${money} dollars", playerName, money)
```

### `utils.debug(...)`
Debug print (only if Config.debug is true).

**Parameters:**
- `...` - Values to print

**Example:**
```lua
-- Debug information
fivem.utils.debug('Player position:', fivem.players.getCoords())
fivem.utils.debug('Vehicle health:', GetVehicleEngineHealth(fivem.vehicles.get()))
```

### `utils.distance(pos1, pos2)`
Calculates the distance between two positions.

**Parameters:**
- `pos1` (table) - First position with x, y, z coordinates
- `pos2` (table) - Second position with x, y, z coordinates

**Returns:**
- `number` - Distance between the positions

**Example:**
```lua
local playerPos = fivem.players.getCoords()
local targetPos = {x = 100.0, y = 200.0, z = 30.0}
local distance = fivem.utils.distance(playerPos, targetPos)
print('Distance to target:', distance)
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
local pi = 3.14159
local rounded = fivem.utils.round(pi, 2) -- Returns 3.14

local money = 1234.5678
local roundedMoney = fivem.utils.round(money, 2) -- Returns 1234.57
```

### `utils.template(template, ...)`
String interpolation with ${variable} syntax.

**Parameters:**
- `template` (string) - Template string with ${variable} placeholders
- `...` - Values to substitute for variables

**Returns:**
- `string` - Interpolated string

**Example:**
```lua
local city = "Los Santos"
local playerCount = 25
local message = fivem.utils.template("Welcome to ${city}! There are ${count} players online.", city, playerCount)
print(message) -- Output: Welcome to Los Santos! There are 25 players online.
```

## Common Utility Patterns

### Time Management
```lua
-- Delay execution
local function delayedExecution(callback, delay)
  Citizen.CreateThread(function()
    fivem.utils.wait(delay)
    callback()
  end)
end

-- Usage
delayedExecution(function()
  print('This runs after 5 seconds')
end, 5000)

-- Periodic execution
local function periodicExecution(callback, interval)
  Citizen.CreateThread(function()
    while true do
      callback()
      fivem.utils.wait(interval)
    end
  end)
end

-- Usage
periodicExecution(function()
  local health = fivem.players.health()
  fivem.utils.debug('Current health:', health)
end, 1000)
```

### Distance Calculations
```lua
-- Check if player is near a location
local function isPlayerNearLocation(targetCoords, radius)
  local playerCoords = fivem.players.getCoords()
  local distance = fivem.utils.distance(playerCoords, targetCoords)
  return distance <= radius
end

-- Find nearest player
local function findNearestPlayer(maxDistance)
  local nearestPlayer = nil
  local nearestDistance = maxDistance or 100.0
  local playerCoords = fivem.players.getCoords()
  
  local playerCount = GetNumberOfPlayers()
  for i = 0, playerCount - 1 do
    local playerId = GetPlayerFromIndex(i)
    if playerId ~= PlayerId() then
      local otherPlayerCoords = fivem.players.getCoords(playerId)
      local distance = fivem.utils.distance(playerCoords, otherPlayerCoords)
      
      if distance < nearestDistance then
        nearestDistance = distance
        nearestPlayer = playerId
      end
    end
  end
  
  return nearestPlayer, nearestDistance
end

-- Usage
local nearestPlayer, distance = findNearestPlayer(50.0)
if nearestPlayer then
  fivem.utils.print("Nearest player is ${distance} units away", fivem.utils.round(distance, 1))
end
```

### String Formatting
```lua
-- Format money with commas
local function formatMoney(amount)
  local formatted = tostring(fivem.utils.round(amount, 0))
  local parts = {}
  
  for i = #formatted, 1, -3 do
    local start = math.max(1, i - 2)
    table.insert(parts, 1, string.sub(formatted, start, i))
  end
  
  return table.concat(parts, ',')
end

-- Format time
local function formatTime(seconds)
  local hours = math.floor(seconds / 3600)
  local minutes = math.floor((seconds % 3600) / 60)
  local secs = seconds % 60
  
  if hours > 0 then
    return fivem.utils.template("${h}:${m}:${s}", 
      string.format("%02d", hours),
      string.format("%02d", minutes),
      string.format("%02d", secs)
    )
  else
    return fivem.utils.template("${m}:${s}",
      string.format("%02d", minutes),
      string.format("%02d", secs)
    )
  end
end

-- Usage
fivem.utils.print("Money: $${amount}", formatMoney(1234567)) -- Output: Money: $1,234,567
fivem.utils.print("Time: ${time}", formatTime(3661)) -- Output: Time: 1:01:01
```

### Data Validation
```lua
-- Validate coordinates
local function isValidCoords(coords)
  return coords and 
         type(coords.x) == 'number' and 
         type(coords.y) == 'number' and 
         type(coords.z) == 'number'
end

-- Validate player ID
local function isValidPlayerId(playerId)
  return playerId and 
         type(playerId) == 'number' and 
         playerId >= 0 and 
         playerId < GetNumberOfPlayers()
end

-- Safe distance calculation
local function safeDistance(pos1, pos2)
  if not isValidCoords(pos1) or not isValidCoords(pos2) then
    fivem.utils.debug('Invalid coordinates provided for distance calculation')
    return 0
  end
  
  return fivem.utils.distance(pos1, pos2)
end
```

### Performance Monitoring
```lua
-- Measure execution time
local function measureExecutionTime(func, ...)
  local startTime = GetGameTimer()
  local result = func(...)
  local endTime = GetGameTimer()
  local executionTime = endTime - startTime
  
  fivem.utils.debug('Function execution time:', executionTime, 'ms')
  return result, executionTime
end

-- Usage
local result, time = measureExecutionTime(function()
  fivem.utils.wait(1000)
  return "Done"
end)

-- Performance logging
local function logPerformance(operation, startTime)
  local endTime = GetGameTimer()
  local duration = endTime - startTime
  
  if duration > 100 then -- Log slow operations
    fivem.utils.debug('Slow operation detected:', operation, 'took', duration, 'ms')
  end
end
```

### Error Handling
```lua
-- Safe function execution
local function safeExecute(func, ...)
  local success, result = pcall(func, ...)
  
  if not success then
    fivem.utils.debug('Function execution failed:', result)
    return nil
  end
  
  return result
end

-- Retry mechanism
local function retryOperation(operation, maxRetries, delay)
  maxRetries = maxRetries or 3
  delay = delay or 1000
  
  for attempt = 1, maxRetries do
    local success, result = pcall(operation)
    
    if success then
      return result
    end
    
    fivem.utils.debug('Attempt', attempt, 'failed:', result)
    
    if attempt < maxRetries then
      fivem.utils.wait(delay)
    end
  end
  
  fivem.utils.debug('Operation failed after', maxRetries, 'attempts')
  return nil
end
```

## Advanced Utility Functions

### Math Utilities
```lua
-- Clamp value between min and max
local function clamp(value, min, max)
  return math.min(math.max(value, min), max)
end

-- Linear interpolation
local function lerp(a, b, t)
  return a + (b - a) * t
end

-- Check if number is in range
local function isInRange(value, min, max)
  return value >= min and value <= max
end

-- Usage
local clampedHealth = clamp(fivem.players.health(), 0, 200)
local interpolatedValue = lerp(0, 100, 0.5) -- Returns 50
```

### Table Utilities
```lua
-- Deep copy table
local function deepCopy(original)
  local copy = {}
  for key, value in pairs(original) do
    if type(value) == 'table' then
      copy[key] = deepCopy(value)
    else
      copy[key] = value
    end
  end
  return copy
end

-- Merge tables
local function mergeTables(t1, t2)
  local result = deepCopy(t1)
  for key, value in pairs(t2) do
    result[key] = value
  end
  return result
end

-- Check if table contains value
local function tableContains(table, value)
  for _, v in pairs(table) do
    if v == value then
      return true
    end
  end
  return false
end
```

### String Utilities
```lua
-- Capitalize first letter
local function capitalize(str)
  return string.upper(string.sub(str, 1, 1)) .. string.sub(str, 2)
end

-- Split string
local function splitString(str, delimiter)
  local result = {}
  local pattern = string.format("([^%s]+)", delimiter)
  for match in string.gmatch(str, pattern) do
    table.insert(result, match)
  end
  return result
end

-- Truncate string
local function truncateString(str, maxLength, suffix)
  suffix = suffix or "..."
  if string.len(str) <= maxLength then
    return str
  end
  return string.sub(str, 1, maxLength - string.len(suffix)) .. suffix
end

-- Usage
local name = capitalize("john") -- Returns "John"
local words = splitString("hello,world,test", ",") -- Returns {"hello", "world", "test"}
local shortText = truncateString("This is a very long text", 10) -- Returns "This is..."
```

## Best Practices

1. **Use appropriate wait times** - Don't use very short waits in loops
2. **Handle errors gracefully** - Always validate inputs before calculations
3. **Use debug prints sparingly** - Only in development or when needed
4. **Cache frequently used values** - Avoid recalculating the same values
5. **Use template strings for readability** - Makes code more maintainable

```lua
-- Good example
local function processPlayerData(playerId)
  if not isValidPlayerId(playerId) then
    fivem.utils.debug('Invalid player ID:', playerId)
    return false
  end
  
  local playerCoords = fivem.players.getCoords(playerId)
  local distance = fivem.utils.distance(fivem.players.getCoords(), playerCoords)
  
  fivem.utils.print("Player ${id} is ${distance} units away", 
    fivem.utils.round(distance, 1))
  
  return true
end
```

## Migration from Traditional Methods

| Traditional | Library Method |
|-------------|----------------|
| `Citizen.Wait(ms)` | `fivem.utils.wait(ms)` |
| `print(...)` | `fivem.utils.print(...)` |
| `GetDistanceBetweenCoords(x1, y1, z1, x2, y2, z2)` | `fivem.utils.distance(pos1, pos2)` |
| `math.floor(num + 0.5)` | `fivem.utils.round(num)` |
| String concatenation | `fivem.utils.template(template, ...)` |

---

**Next:** [Class System](Class-System.md) | **Previous:** [Database](Database.md) 