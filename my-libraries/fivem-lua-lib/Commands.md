# Commands & KeyMapping

The Commands and KeyMapping modules provide simplified APIs for registering commands and key bindings in FiveM.

## Compatibility
✅ **Commands**: Shared - Works on both client and server-side  
🖥️ **KeyMapping**: Client-Only - Only works on client-side

## Commands Module

### Basic Usage

```lua
-- Register a command
fivem.commands:reg('heal', function(source, args)
  if source == 0 then -- Console command
    print('Heal command executed from console')
  else
    print('Heal command executed by player:', source)
  end
end, false)

-- Or use global access (no fivem prefix needed)
commands:reg('heal', function(source, args)
  print('Heal command executed!')
end, false)
```

### Available Methods

#### `commands:reg(name, handler, restricted)`
Registers a new command.

**Parameters:**
- `name` (string) - The command name (without the `/`)
- `handler` (function) - The function to execute when command is called
- `restricted` (boolean, optional) - Whether the command is restricted (default: false)

**Example:**
```lua
-- Basic command
commands:reg('hello', function(source, args)
  print('Hello command executed by:', source)
end)

-- Restricted command (admin only)
commands:reg('admin', function(source, args)
  if source == 0 then -- Console
    print('Admin command from console')
  else
    print('Admin command from player:', source)
  end
end, true)
```

#### `commands:suggest(name, help, params)`
Adds a chat suggestion for a command.

**Parameters:**
- `name` (string) - The command name
- `help` (string) - Help text for the command
- `params` (table, optional) - Parameter suggestions

**Example:**
```lua
-- Add suggestion
commands:suggest('tp', 'Teleport to coordinates', {
  { name = 'x', help = 'X coordinate' },
  { name = 'y', help = 'Y coordinate' },
  { name = 'z', help = 'Z coordinate' }
})

-- Remove suggestion
commands:remove('tp')
```

#### `commands:remove(name)`
Removes a command suggestion.

**Parameters:**
- `name` (string) - The command name to remove

## KeyMapping Module

### Basic Usage

```lua
-- Register a key mapping
fivem.keyMapping:reg('showinfo', 'Show Player Info', 'keyboard', 'F1')

-- Or use global access (no fivem prefix needed)
keyMapping:reg('showinfo', 'Show Player Info', 'keyboard', 'F1')
```

### Available Methods

#### `keyMapping:reg(commandName, description, defaultMapper, defaultParameter)`
Registers a key mapping for a command.

**Parameters:**
- `commandName` (string) - The command name to bind
- `description` (string) - Description of the key binding
- `defaultMapper` (string, optional) - Input mapper (default: 'keyboard')
- `defaultParameter` (string, optional) - Default key (default: '')

**Example:**
```lua
-- Basic key mapping
keyMapping:reg('heal', 'Heal Player', 'keyboard', 'H')

-- Mouse button mapping
keyMapping:reg('shoot', 'Shoot Weapon', 'mouse_button', 'MOUSE_LEFT')

-- Controller mapping
keyMapping:reg('jump', 'Jump', 'pad_digital', 'PAD_0')
```

#### `keyMapping:remove(commandName)`
Removes a key mapping.

**Parameters:**
- `commandName` (string) - The command name to unbind

## Complete Examples

### Server-Side Commands

```lua
-- Admin commands
commands:reg('healall', function(source, args)
  if source == 0 then -- Console only
    for _, playerId in ipairs(GetPlayers()) do
      players:setHp(tonumber(playerId), 200)
    end
    print('All players healed!')
  else
    print('This command can only be used from console')
  end
end, true)

commands:reg('tpplayer', function(source, args)
  if source == 0 and #args >= 4 then
    local targetName = args[1]
    local x, y, z = tonumber(args[2]), tonumber(args[3]), tonumber(args[4])
    
    -- Find player by name and teleport
    for _, playerId in ipairs(GetPlayers()) do
      if GetPlayerName(playerId) == targetName then
        players:tp(playerId, {x = x, y = y, z = z})
        print('Player', targetName, 'teleported to', x, y, z)
        break
      end
    end
  else
    print('Usage: tpplayer <name> <x> <y> <z> (console only)')
  end
end, true)

-- Add suggestions
commands:suggest('healall', 'Heal all players (console only)')
commands:suggest('tpplayer', 'Teleport player to coordinates', {
  { name = 'name', help = 'Player name' },
  { name = 'x', help = 'X coordinate' },
  { name = 'y', help = 'Y coordinate' },
  { name = 'z', help = 'Z coordinate' }
})
```

### Client-Side Commands & KeyMappings

```lua
-- Client commands
commands:reg('heal', function(source, args)
  players:setHp(200)
  print('You have been healed!')
end, false)

commands:reg('tp', function(source, args)
  if #args >= 3 then
    local x, y, z = tonumber(args[1]), tonumber(args[2]), tonumber(args[3])
    if x and y and z then
      players:tp({x = x, y = y, z = z})
      print('Teleported to:', x, y, z)
    else
      print('Invalid coordinates!')
    end
  else
    print('Usage: /tp <x> <y> <z>')
  end
end, false)

commands:reg('car', function(source, args)
  if #args >= 1 then
    local model = args[1]
    local coords = players:pos()
    local vehicle = vehicles:spawn(model, coords)
    
    if vehicle ~= 0 then
      SetPedIntoVehicle(players:get(), vehicle, -1)
      print('Vehicle spawned:', model)
    else
      print('Failed to spawn vehicle:', model)
    end
  else
    print('Usage: /car <model>')
  end
end, false)

-- Key mappings
keyMapping:reg('heal', 'Heal Player', 'keyboard', 'H')
keyMapping:reg('showinfo', 'Show Player Info', 'keyboard', 'F1')
keyMapping:reg('vehicleinfo', 'Show Vehicle Info', 'keyboard', 'F2')

-- Add suggestions
commands:suggest('heal', 'Heal yourself')
commands:suggest('tp', 'Teleport to coordinates', {
  { name = 'x', help = 'X coordinate' },
  { name = 'y', help = 'Y coordinate' },
  { name = 'z', help = 'Z coordinate' }
})
commands:suggest('car', 'Spawn a vehicle', {
  { name = 'model', help = 'Vehicle model name' }
})
```

### Environment Detection

```lua
-- Check environment before registering
if fivem.isClient then
  -- Client-side commands and key mappings
  commands:reg('clientcmd', function(source, args)
    print('Client command executed!')
  end)
  
  keyMapping:reg('clientkey', 'Client Key Binding', 'keyboard', 'F3')
else
  -- Server-side commands only
  commands:reg('servercmd', function(source, args)
    print('Server command executed by:', source)
  end, true)
end
```

## Best Practices

1. **Use descriptive command names** - Make command names clear and intuitive
2. **Add help text** - Always provide helpful descriptions for commands
3. **Check permissions** - Use the `restricted` parameter for admin commands
4. **Validate input** - Always validate command arguments
5. **Environment awareness** - Use `fivem.isClient` and `fivem.isServer` when needed
6. **Clean up** - Remove commands and key mappings when resource stops

```lua
-- Good example with validation
commands:reg('teleport', function(source, args)
  if #args < 3 then
    print('Usage: /teleport <x> <y> <z>')
    return
  end
  
  local x, y, z = tonumber(args[1]), tonumber(args[2]), tonumber(args[3])
  if not x or not y or not z then
    print('Invalid coordinates provided')
    return
  end
  
  players:tp({x = x, y = y, z = z})
  print('Teleported to:', x, y, z)
end)

-- Clean up on resource stop
events:on('onResourceStop', function(resourceName)
  if resourceName == GetCurrentResourceName() then
    commands:remove('teleport')
    if fivem.isClient then
      keyMapping:remove('teleport')
    end
  end
end)
```

## Migration from Traditional Commands

| Traditional | Library Method |
|-------------|----------------|
| `RegisterCommand('name', handler, restricted)` | `fivem.commands:reg('name', handler, restricted)` or `commands:reg('name', handler, restricted)` |
| `RegisterKeyMapping('command', 'description', 'mapper', 'key')` | `fivem.keyMapping:reg('command', 'description', 'mapper', 'key')` or `keyMapping:reg('command', 'description', 'mapper', 'key')` |
| `TriggerEvent('chat:addSuggestion', 'name', 'help', params)` | `fivem.commands:suggest('name', 'help', params)` or `commands:suggest('name', 'help', params)` |

---

**Next:** [Utilities](Utilities.md) | **Previous:** [Vehicles](Vehicles.md) 