# Vehicles

The Vehicles module provides simplified methods for spawning, managing, and controlling vehicles in FiveM.

## Compatibility
🖥️ **Client-Only** - Works only on client-side

- **Client-side**: Full vehicle functionality
- **Server-side**: Functions return warnings and default values

## Basic Usage

```lua
-- Get current vehicle
local vehicle = fivem.vehicles.get()

-- Or use global access (no fivem prefix needed)
local vehicle = vehicles.get()

-- Spawn vehicle
local newVehicle = fivem.vehicles.spawn('adder', {x = 0.0, y = 0.0, z = 30.0})

-- Or use global access
local newVehicle = vehicles.spawn('adder', {x = 0.0, y = 0.0, z = 30.0})

-- Delete vehicle
fivem.vehicles.del(vehicle)

-- Or use global access
vehicles.del(vehicle)
```

## Available Methods

### `vehicles.get()`
Gets the current vehicle the player is in.

**Returns:**
- `number` - The vehicle entity, or 0 if not in a vehicle

**Example:**
```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  print('Currently in vehicle:', vehicle)
else
  print('Not in a vehicle')
end

-- Or use global access
local vehicle = vehicles.get()
if vehicle ~= 0 then
  print('Currently in vehicle:', vehicle)
else
  print('Not in a vehicle')
end
```

### `vehicles.spawn(model, coords)`
Spawns a vehicle at the specified coordinates.

**Parameters:**
- `model` (string) - The vehicle model name (e.g., 'adder', 'zentorno')
- `coords` (table) - Coordinates table with x, y, z properties

**Returns:**
- `number` - The spawned vehicle entity

**Example:**
```lua
local vehicle = fivem.vehicles.spawn('adder', {x = 100.0, y = 200.0, z = 30.0})
if vehicle ~= 0 then
  print('Vehicle spawned successfully')
end

-- Or use global access
local vehicle = vehicles.spawn('adder', {x = 100.0, y = 200.0, z = 30.0})
if vehicle ~= 0 then
  print('Vehicle spawned successfully')
end
```

### `vehicles.del(vehicle)`
Deletes a vehicle entity.

**Parameters:**
- `vehicle` (number) - The vehicle entity to delete

**Example:**
```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  fivem.vehicles.del(vehicle)
  print('Vehicle deleted')
end

-- Or use global access
local vehicle = vehicles.get()
if vehicle ~= 0 then
  vehicles.del(vehicle)
  print('Vehicle deleted')
end
```

### `vehicles.pos(vehicle)`
Gets the vehicle's coordinates.

**Parameters:**
- `vehicle` (number) - The vehicle entity

**Returns:**
- `table` - Coordinates table with x, y, z properties

**Example:**
```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  local coords = fivem.vehicles.pos(vehicle)
  print('Vehicle position:', coords.x, coords.y, coords.z)
end

-- Or use global access
local vehicle = vehicles.get()
if vehicle ~= 0 then
  local coords = vehicles.pos(vehicle)
  print('Vehicle position:', coords.x, coords.y, coords.z)
end
```

### `vehicles.tp(vehicle, coords)`
Teleports a vehicle to the specified coordinates.

**Parameters:**
- `vehicle` (number) - The vehicle entity
- `coords` (table) - Coordinates table with x, y, z properties

**Example:**
```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  fivem.vehicles.tp(vehicle, {x = 500.0, y = 500.0, z = 50.0})
end

-- Or use global access
local vehicle = vehicles.get()
if vehicle ~= 0 then
  vehicles.tp(vehicle, {x = 500.0, y = 500.0, z = 50.0})
end
```

## Common Vehicle Operations

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

-- Usage
local vehicle = spawnVehicleForPlayer('adder', fivem.players.pos())

-- Or use global access
local vehicle = spawnVehicleForPlayer('adder', players.pos())
```

### Vehicle Management
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
end

-- Or use global access
local function cleanupVehicles()
  for _, vehicle in ipairs(spawnedVehicles) do
    if DoesEntityExist(vehicle) then
      vehicles.del(vehicle)
    end
  end
  spawnedVehicles = {}
end
```

### Vehicle Monitoring
```lua
-- Monitor vehicle health
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local vehicle = fivem.vehicles.get()
    
    if vehicle ~= 0 then
      local health = GetVehicleEngineHealth(vehicle)
      if health < 500 then
        print('Warning: Vehicle engine health low!')
      end
    end
  end
end)

-- Or use global access
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local vehicle = vehicles.get()
    
    if vehicle ~= 0 then
      local health = GetVehicleEngineHealth(vehicle)
      if health < 500 then
        print('Warning: Vehicle engine health low!')
      end
    end
  end
end)
```

### Vehicle Customization
```lua
-- Basic vehicle customization
local function customizeVehicle(vehicle, color, plate)
  if vehicle == 0 then return false end
  
  -- Set color
  if color then
    SetVehicleColours(vehicle, color.primary, color.secondary or color.primary)
  end
  
  -- Set plate
  if plate then
    SetVehicleNumberPlateText(vehicle, plate)
  end
  
  -- Set as mission entity (won't be deleted by game)
  SetEntityAsMissionEntity(vehicle, true, true)
  
  return true
end

-- Usage
local vehicle = fivem.vehicles.spawn('adder', fivem.players.pos())
customizeVehicle(vehicle, {primary = 0, secondary = 1}, 'MYPLATE')

-- Or use global access
local vehicle = vehicles.spawn('adder', players.pos())
customizeVehicle(vehicle, {primary = 0, secondary = 1}, 'MYPLATE')
```

## Client-Side Vehicle Examples

```lua
-- Client-side vehicle spawning
fivem.events.on('spawnVehicle', function(model)
  local coords = fivem.players.pos()
  local vehicle = fivem.vehicles.spawn(model, coords)
  
  if vehicle ~= 0 then
    -- Put player in vehicle
    SetPedIntoVehicle(fivem.players.get(), vehicle, -1)
    print('Vehicle spawned!')
  end
end)

-- Or use global access
events.on('spawnVehicle', function(model)
  local coords = players.pos()
  local vehicle = vehicles.spawn(model, coords)
  
  if vehicle ~= 0 then
    -- Put player in vehicle
    SetPedIntoVehicle(players.get(), vehicle, -1)
    print('Vehicle spawned!')
  end
end)

-- Client-side vehicle cleanup
fivem.events.on('deleteVehicle', function()
  local vehicle = fivem.vehicles.get()
  if vehicle ~= 0 then
    fivem.vehicles.del(vehicle)
    print('Vehicle deleted!')
  end
end)

-- Or use global access
events.on('deleteVehicle', function()
  local vehicle = vehicles.get()
  if vehicle ~= 0 then
    vehicles.del(vehicle)
    print('Vehicle deleted!')
  end
end)

-- Client-side vehicle monitoring
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local vehicle = fivem.vehicles.get()
    
    if vehicle ~= 0 then
      local speed = GetEntitySpeed(vehicle) * 3.6 -- km/h
      local fuel = GetVehicleFuelLevel(vehicle)
      local health = GetVehicleEngineHealth(vehicle)
      
      -- Log vehicle info
      print('Vehicle speed:', math.floor(speed), 'km/h')
      print('Vehicle fuel:', fuel)
      print('Vehicle health:', health)
    end
  end
end)

-- Or use global access
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(1000)
    local vehicle = vehicles.get()
    
    if vehicle ~= 0 then
      local speed = GetEntitySpeed(vehicle) * 3.6 -- km/h
      local fuel = GetVehicleFuelLevel(vehicle)
      local health = GetVehicleEngineHealth(vehicle)
      
      -- Log vehicle info
      print('Vehicle speed:', math.floor(speed), 'km/h')
      print('Vehicle fuel:', fuel)
      print('Vehicle health:', health)
    end
  end
end)
```

## Server-Side Vehicle Limitations

When using vehicle functions on the server-side, you'll get warnings:

```lua
-- Server-side (will show warnings)
local vehicle = fivem.vehicles.get() -- Warning: vehicles.get() is client-only
local newVehicle = fivem.vehicles.spawn('adder', {x=0, y=0, z=30}) -- Warning: vehicles.spawn() is client-only

-- Or use global access
local vehicle = vehicles.get() -- Warning: vehicles.get() is client-only
local newVehicle = vehicles.spawn('adder', {x=0, y=0, z=30}) -- Warning: vehicles.spawn() is client-only
```

To work with vehicles from the server, use events to communicate with clients:

```lua
-- Server-side vehicle spawning
fivem.events.on('serverSpawnVehicle', function(model, coords)
  local playerId = source
  fivem.events.emitClient('spawnVehicle', playerId, model, coords)
end)

-- Or use global access
events.on('serverSpawnVehicle', function(model, coords)
  local playerId = source
  events.emitClient('spawnVehicle', playerId, model, coords)
end)

-- Server-side vehicle deletion
fivem.events.on('serverDeleteVehicle', function()
  local playerId = source
  fivem.events.emitClient('deleteVehicle', playerId)
end)

-- Or use global access
events.on('serverDeleteVehicle', function()
  local playerId = source
  events.emitClient('deleteVehicle', playerId)
end)
```

## Popular Vehicle Models

### Sports Cars
```lua
local sportsCars = {
  'adder',      -- Bugatti Veyron
  'zentorno',   -- Lamborghini Veneno
  't20',        -- McLaren P1
  'osiris',     -- Pagani Huayra
  'entityxf',   -- Koenigsegg Agera
  'fmj',        -- Aston Martin Vulcan
  'reaper',     -- LaFerrari
  'xa21'        -- Jaguar C-X75
}
```

### SUVs and Trucks
```lua
local suvs = {
  'baller',     -- Range Rover
  'cavalcade',  -- Cadillac Escalade
  'dubsta',     -- Mercedes G-Class
  'granger',    -- Chevrolet Suburban
  'patrol',     -- Police SUV
  'seminole'    -- Jeep Grand Cherokee
}
```

### Motorcycles
```lua
local motorcycles = {
  'akuma',      -- Suzuki Hayabusa
  'bati',       -- Ducati 1199
  'hakuchou',   -- Suzuki Hayabusa
  'pcj',        -- Honda CBR1000RR
  'ruffian',    -- Yamaha FZ-09
  'sanchez'     -- Honda CRF450R
}
```

## Vehicle Spawning Best Practices

### Model Loading
```lua
-- Always load models before spawning
local function loadVehicleModel(model)
  local hash = GetHashKey(model)
  RequestModel(hash)
  
  local timeout = 0
  while not HasModelLoaded(hash) and timeout < 100 do
    Citizen.Wait(10)
    timeout = timeout + 1
  end
  
  if HasModelLoaded(hash) then
    return hash
  else
    print('Failed to load model:', model)
    return nil
  end
end

-- Safe vehicle spawning
local function safeSpawnVehicle(model, coords)
  local hash = loadVehicleModel(model)
  if not hash then return 0 end
  
  local vehicle = fivem.vehicles.spawn(model, coords)
  SetModelAsNoLongerNeeded(hash)
  
  return vehicle
end

-- Or use global access
local function safeSpawnVehicle(model, coords)
  local hash = loadVehicleModel(model)
  if not hash then return 0 end
  
  local vehicle = vehicles.spawn(model, coords)
  SetModelAsNoLongerNeeded(hash)
  
  return vehicle
end
```

### Coordinate Validation
```lua
-- Check if coordinates are valid for vehicle spawning
local function isValidSpawnLocation(coords)
  -- Check if there's ground at the location
  local ground, groundZ = GetGroundZFor_3dCoord(coords.x, coords.y, coords.z, false)
  
  if not ground then
    return false
  end
  
  -- Check if there are any entities in the way
  local clearArea = IsAreaClearOfEntities(coords.x, coords.y, coords.z, 5.0, false, true, true, true, false, 0, false)
  
  return clearArea
end
```

### Error Handling
```lua
-- Comprehensive vehicle spawning with error handling
local function spawnVehicleWithValidation(model, coords)
  -- Validate inputs
  if not model or not coords then
    print('Invalid model or coordinates')
    return 0
  end
  
  -- Check spawn location
  if not isValidSpawnLocation(coords) then
    print('Invalid spawn location')
    return 0
  end
  
  -- Spawn vehicle
  local vehicle = safeSpawnVehicle(model, coords)
  
  if vehicle == 0 then
    print('Failed to spawn vehicle')
    return 0
  end
  
  -- Verify vehicle was created properly
  if not DoesEntityExist(vehicle) then
    print('Vehicle entity does not exist after spawning')
    return 0
  end
  
  return vehicle
end
```

## Best Practices

1. **Always check if vehicle exists** before performing operations
2. **Load models properly** before spawning vehicles
3. **Clean up vehicles** when they're no longer needed
4. **Validate coordinates** before spawning vehicles
5. **Use client-side only** for vehicle operations

```lua
-- Good example
local function safeVehicleOperation(operation, ...)
  if not fivem.isClient then
    print('Vehicle operations are client-only')
    return false
  end
  
  local success, result = pcall(operation, ...)
  
  if not success then
    print('Vehicle operation failed:', result)
    return false
  end
  
  return true
end

-- Or use global access
local function safeVehicleOperation(operation, ...)
  if not fivem.isClient then
    print('Vehicle operations are client-only')
    return false
  end
  
  local success, result = pcall(operation, ...)
  
  if not success then
    print('Vehicle operation failed:', result)
    return false
  end
  
  return true
end

-- Usage
safeVehicleOperation(function()
  local vehicle = fivem.vehicles.spawn('adder', {x=0, y=0, z=30})
  return vehicle
end)

-- Or use global access
safeVehicleOperation(function()
  local vehicle = vehicles.spawn('adder', {x=0, y=0, z=30})
  return vehicle
end)
```

## Migration from Traditional Methods

| Traditional | Library Method |
|-------------|----------------|
| `GetVehiclePedIsIn(GetPlayerPed(-1), false)` | `fivem.vehicles.get()` or `vehicles.get()` |
| `CreateVehicle(hash, x, y, z, heading, true, false)` | `fivem.vehicles.spawn(model, coords)` or `vehicles.spawn(model, coords)` |
| `DeleteEntity(vehicle)` | `fivem.vehicles.del(vehicle)` or `vehicles.del(vehicle)` |
| `GetEntityCoords(vehicle)` | `fivem.vehicles.pos(vehicle)` or `vehicles.pos(vehicle)` |
| `SetEntityCoords(vehicle, x, y, z)` | `fivem.vehicles.tp(vehicle, coords)` or `vehicles.tp(vehicle, coords)` |
