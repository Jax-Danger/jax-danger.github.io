# Events

The Events module provides a simplified API for handling FiveM events, replacing the traditional `AddEventHandler` with cleaner, more readable methods.

## Compatibility

✅ **Shared** - Works on both client and server-side

## Basic Usage

Instead of using `AddEventHandler`, use the simplified events API:

```lua
-- Using namespace
fivem.events:on('playerSpawned', function()
  print('Player spawned!')
end)

-- Using global access (no fivem prefix needed)
events:on('playerSpawned', function()
  print('Player spawned!')
end)
```

## Available Methods

### `events.on(eventName, callback)`

Adds an event handler for the specified event.

**Parameters:**

* `eventName` (string) - The name of the event to listen for
* `callback` (function) - The function to execute when the event is triggered

**Example:**

```lua
fivem.events:on('playerSpawned', function()
  print('Player has spawned!')
end)

-- Or use global access
events:on('playerSpawned', function()
  print('Player has spawned!')
end)
```

### `events:emit(eventName, ...)`

Triggers a local event.

**Parameters:**

* `eventName` (string) - The name of the event to trigger
* `...` - Additional arguments to pass to the event handlers

**Example:**

```lua
fivem.events:emit('customEvent', 'Hello', 'World')

-- Or use global access
events:emit('customEvent', 'Hello', 'World')
```

### `events.emitServer(eventName, ...)`

Triggers a server event.

**Parameters:**

* `eventName` (string) - The name of the event to trigger on the server
* `...` - Additional arguments to pass to the server

**Example:**

```lua
fivem.events.emitServer('savePlayerData', playerData)

-- Or use global access
events.emitServer('savePlayerData', playerData)
```

### `events.emitClient(eventName, target, ...)`

Triggers a client event for a specific player.

**Parameters:**

* `eventName` (string) - The name of the event to trigger
* `target` (number) - The player ID to send the event to
* `...` - Additional arguments to pass to the client

**Example:**

```lua
fivem.events:emitClient('showNotification', playerId, 'Welcome!')

-- Or use global access
events:emitClient('showNotification', playerId, 'Welcome!')
```

## Best Practices

1. **Use descriptive event names** - Make event names clear and specific
2. **Handle errors** - Always check for errors in event callbacks
3. **Clean up events** - Remove event handlers when they're no longer needed
4. **Use namespacing** - Consider prefixing custom events with your resource name
5. **Check environment** - Use `fivem.isServer` or `fivem.isClient` when needed

```lua
-- Good example
events:on('myResource:playerJoined', function(playerId)
  -- Handle player join
end)

-- Clean up when resource stops
events:on('onResourceStop', function(resourceName)
  if resourceName == GetCurrentResourceName() then
    events:off('myResource:playerJoined')
  end
end)

-- Environment-specific handling
events:on('playerSpawned', function()
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

| Traditional                                    | Library Method                                                                                       |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `AddEventHandler('eventName', callback)`       | `fivem.events:on('eventName', callback)` or `events:on('eventName', callback)`                       |
| `TriggerEvent('eventName', ...)`               | `fivem.events:emit('eventName', ...)` or `events:emit('eventName', ...)`                             |
| `TriggerServerEvent('eventName', ...)`         | `fivem.events:emitServer('eventName', ...)` or `events:emitServer('eventName', ...)`                 |
| `TriggerClientEvent('eventName', target, ...)` | `fivem.events:emitClient('eventName', target, ...)` or `events:emitClient('eventName', target, ...)` |
