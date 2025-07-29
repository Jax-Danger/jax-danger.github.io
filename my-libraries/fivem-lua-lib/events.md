# Events

The Events module provides a simplified API for handling FiveM events, replacing the traditional `AddEventHandler` with cleaner, more readable methods.

## Compatibility
✅ **Shared** - Works on both client and server-side

## Basic Usage

Instead of using `AddEventHandler`, use the simplified events API:

```lua
-- Using namespace
fivem.events.on('playerSpawned', function()
  print('Player spawned!')
end)

-- Using global access (no fivem prefix needed)
events.on('playerSpawned', function()
  print('Player spawned!')
end)
```

## Available Methods

### `events.on(eventName, callback)`
Adds an event handler for the specified event.

**Parameters:**
- `eventName` (string) - The name of the event to listen for
- `callback` (function) - The function to execute when the event is triggered

**Example:**
```lua
fivem.events.on('playerSpawned', function()
  print('Player has spawned!')
end)

-- Or use global access
events.on('playerSpawned', function()
  print('Player has spawned!')
end)
```

### `events.mod(eventName, callback)`
Modifies an existing event handler.

**Parameters:**
- `eventName` (string) - The name of the event to modify
- `callback` (function) - The new function to execute

**Example:**
```lua
fivem.events.mod('playerSpawned', function()
  print('Modified spawn handler!')
end)

-- Or use global access
events.mod('playerSpawned', function()
  print('Modified spawn handler!')
end)
```

### `events.off(eventName)`
Removes an event handler.

**Parameters:**
- `eventName` (string) - The name of the event to remove

**Example:**
```lua
fivem.events.off('playerSpawned')

-- Or use global access
events.off('playerSpawned')
```

### `events.emit(eventName, ...)`
Triggers a local event.

**Parameters:**
- `eventName` (string) - The name of the event to trigger
- `...` - Additional arguments to pass to the event handlers

**Example:**
```lua
fivem.events.emit('customEvent', 'Hello', 'World')

-- Or use global access
events.emit('customEvent', 'Hello', 'World')
```

### `events.emitServer(eventName, ...)`
Triggers a server event.

**Parameters:**
- `eventName` (string) - The name of the event to trigger on the server
- `...` - Additional arguments to pass to the server

**Example:**
```lua
fivem.events.emitServer('savePlayerData', playerData)

-- Or use global access
events.emitServer('savePlayerData', playerData)
```

### `events.emitClient(eventName, target, ...)`
Triggers a client event for a specific player.

**Parameters:**
- `eventName` (string) - The name of the event to trigger
- `target` (number) - The player ID to send the event to
- `...` - Additional arguments to pass to the client

**Example:**
```lua
fivem.events.emitClient('showNotification', playerId, 'Welcome!')

-- Or use global access
events.emitClient('showNotification', playerId, 'Welcome!')
```

## Common Event Examples

### Player Events
```lua
-- Player spawn
fivem.events.on('playerSpawned', function()
  print('Player spawned!')
end)

-- Or use global access
events.on('playerSpawned', function()
  print('Player spawned!')
end)

-- Player death
events.on('playerDied', function()
  print('Player died!')
end)

-- Player entering vehicle
events.on('playerEnteredVehicle', function(vehicle, seat)
  print('Player entered vehicle:', vehicle)
end)
```

### Resource Events
```lua
-- Resource start
events.on('onResourceStart', function(resourceName)
  if resourceName == GetCurrentResourceName() then
    print('Resource started!')
  end
end)

-- Resource stop
events.on('onResourceStop', function(resourceName)
  if resourceName == GetCurrentResourceName() then
    print('Resource stopping!')
  end
end)
```

### Custom Events
```lua
-- Define custom event
events.on('myCustomEvent', function(data)
  print('Custom event received:', data)
end)

-- Trigger custom event
events.emit('myCustomEvent', {message = 'Hello World'})
```

## Server-Side Event Examples

```lua
-- Server-side player connection handling
events.on('playerConnecting', function(name, setKickReason, deferrals)
  print('Player connecting:', name)
  -- Handle player connection logic
end)

-- Server-side player disconnection
events.on('playerDropped', function(reason)
  local playerId = source
  print('Player disconnected:', playerId, 'Reason:', reason)
  -- Clean up player data
end)

-- Server-side custom event
events.on('playerRequestMoney', function(amount)
  local playerId = source
  -- Handle money request logic
  events.emitClient('moneyResponse', playerId, true, amount)
end)
```

## Client-Side Event Examples

```lua
-- Client-side spawn handling
events.on('playerSpawned', function()
  -- Set up player on spawn
  players.setHp(200)
end)

-- Client-side custom event
events.on('showNotification', function(message)
  print('Notification:', message)
end)

-- Request money from server
events.emitServer('playerRequestMoney', 1000)
```

## Best Practices

1. **Use descriptive event names** - Make event names clear and specific
2. **Handle errors** - Always check for errors in event callbacks
3. **Clean up events** - Remove event handlers when they're no longer needed
4. **Use namespacing** - Consider prefixing custom events with your resource name
5. **Check environment** - Use `fivem.isServer` or `fivem.isClient` when needed

```lua
-- Good example
events.on('myResource:playerJoined', function(playerId)
  -- Handle player join
end)

-- Clean up when resource stops
events.on('onResourceStop', function(resourceName)
  if resourceName == GetCurrentResourceName() then
    events.off('myResource:playerJoined')
  end
end)

-- Environment-specific handling
events.on('playerSpawned', function()
  if fivem.isClient then
    -- Client-side spawn logic
    print('Client spawn logic')
  else
    -- Server-side spawn logic
    print('Player spawned on server')
  end
end)
```

## Migration from Traditional Events

| Traditional | Library Method |
|-------------|----------------|
| `AddEventHandler('eventName', callback)` | `fivem.events.on('eventName', callback)` or `events.on('eventName', callback)` |
| `TriggerEvent('eventName', ...)` | `fivem.events.emit('eventName', ...)` or `events.emit('eventName', ...)` |
| `TriggerServerEvent('eventName', ...)` | `fivem.events.emitServer('eventName', ...)` or `events.emitServer('eventName', ...)` |
| `TriggerClientEvent('eventName', target, ...)` | `fivem.events.emitClient('eventName', target, ...)` or `events.emitClient('eventName', target, ...)` |

---

**Next:** [Players](Players.md) | **Previous:** [Home](Home.md) 