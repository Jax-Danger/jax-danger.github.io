# Vehicles

The Vehicles module provides simplified methods for spawning, managing, and controlling vehicles in FiveM.

## Compatibility

🖥️ **Client-Only** - Works only on client-side

* **Client-side**: Full vehicle functionality

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

* `number` - The vehicle entity, or 0 if not in a vehicle

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

-- Or use global access
local vehicle = vehicles.spawn('adder', {x = 100.0, y = 200.0, z = 30.0})
if vehicle ~= 0 then
  print('Vehicle spawned successfully')
end
```

### `vehicles.del(vehicle)`

Deletes a vehicle entity.

**Parameters:**

* `vehicle` (number) - The vehicle entity to delete

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

* `vehicle` (number) - The vehicle entity

**Returns:**

* `table` - Coordinates table with x, y, z properties

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

* `vehicle` (number) - The vehicle entity
* `coords` (table) - Coordinates table with x, y, z properties

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

## Best Practices

1. **Always check if vehicle exists** before performing operations
2. **Load models properly** before spawning vehicles
3. **Clean up vehicles** when they're no longer needed
4. **Validate coordinates** before spawning vehicles
5. **Use client-side only** for vehicle operations

## Migration from Traditional Methods

| Traditional                                          | Library Method                                                           |
| ---------------------------------------------------- | ------------------------------------------------------------------------ |
| `GetVehiclePedIsIn(GetPlayerPed(-1), false)`         | `fivem.vehicles.get()` or `vehicles.get()`                               |
| `CreateVehicle(hash, x, y, z, heading, true, false)` | `fivem.vehicles.spawn(model, coords)` or `vehicles.spawn(model, coords)` |
| `DeleteEntity(vehicle)`                              | `fivem.vehicles.del(vehicle)` or `vehicles.del(vehicle)`                 |
| `GetEntityCoords(vehicle)`                           | `fivem.vehicles.pos(vehicle)` or `vehicles.pos(vehicle)`                 |
| `SetEntityCoords(vehicle, x, y, z)`                  | `fivem.vehicles.tp(vehicle, coords)` or `vehicles.tp(vehicle, coords)`   |
