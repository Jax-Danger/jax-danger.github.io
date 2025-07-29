# Vehicles

The Vehicles module provides simplified methods for spawning, managing, and controlling vehicles in FiveM.

## Basic Usage

```lua
-- Get current vehicle
local vehicle = fivem.vehicles.get()

-- Spawn vehicle
local newVehicle = fivem.vehicles.spawn('adder', {x = 0.0, y = 0.0, z = 30.0})

-- Delete vehicle
fivem.vehicles.delete(vehicle)
```

## Available Methods

### `vehicles.get()`

Gets the current vehicle the player is in.

**Returns:**

* `number` - The vehicle entity, or 0 if not in a vehicle

**Example:**

```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  print('Currently in vehicle:', vehicle)
else
  print('Not in a vehicle')
end
```

### `vehicles.spawn(model, coords)`

Spawns a vehicle at the specified coordinates.

**Parameters:**

* `model` (string) - The vehicle model name (e.g., 'adder', 'zentorno')
* `coords` (table) - Coordinates table with x, y, z properties

**Returns:**

* `number` - The spawned vehicle entity

**Example:**

```lua
local vehicle = fivem.vehicles.spawn('adder', {x = 100.0, y = 200.0, z = 30.0})
if vehicle ~= 0 then
  print('Vehicle spawned successfully')
end
```

### `vehicles.delete(vehicle)`

Deletes a vehicle entity.

**Parameters:**

* `vehicle` (number) - The vehicle entity to delete

**Example:**

```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  fivem.vehicles.delete(vehicle)
  print('Vehicle deleted')
end
```

### `vehicles.getCoords(vehicle)`

Gets the vehicle's coordinates.

**Parameters:**

* `vehicle` (number) - The vehicle entity

**Returns:**

* `table` - Coordinates table with x, y, z properties

**Example:**

```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  local coords = fivem.vehicles.getCoords(vehicle)
  print('Vehicle position:', coords.x, coords.y, coords.z)
end
```

### `vehicles.teleport(vehicle, coords)`

Teleports a vehicle to the specified coordinates.

**Parameters:**

* `vehicle` (number) - The vehicle entity
* `coords` (table) - Coordinates table with x, y, z properties

**Example:**

```lua
local vehicle = fivem.vehicles.get()
if vehicle ~= 0 then
  fivem.vehicles.teleport(vehicle, {x = 500.0, y = 500.0, z = 50.0})
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

-- Usage
local vehicle = spawnVehicleForPlayer('adder', fivem.players.getCoords())
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

-- Clean up all spawned vehicles
local function cleanupVehicles()
  for _, vehicle in ipairs(spawnedVehicles) do
    if DoesEntityExist(vehicle) then
      fivem.vehicles.delete(vehicle)
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
local vehicle = fivem.vehicles.spawn('adder', fivem.players.getCoords())
customizeVehicle(vehicle, {primary = 0, secondary = 1}, 'MYPLATE')
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

## Migration from Traditional Methods

| Traditional                                          | Library Method                             |
| ---------------------------------------------------- | ------------------------------------------ |
| `GetVehiclePedIsIn(GetPlayerPed(-1), false)`         | `fivem.vehicles.get()`                     |
| `CreateVehicle(hash, x, y, z, heading, true, false)` | `fivem.vehicles.spawn(model, coords)`      |
| `DeleteEntity(vehicle)`                              | `fivem.vehicles.delete(vehicle)`           |
| `GetEntityCoords(vehicle)`                           | `fivem.vehicles.getCoords(vehicle)`        |
| `SetEntityCoords(vehicle, x, y, z)`                  | `fivem.vehicles.teleport(vehicle, coords)` |

***

**Next:** [Utilities](utilities.md) | **Previous:** [Players](players.md)
