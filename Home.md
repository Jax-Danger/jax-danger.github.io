# FiveM Lua Library Documentation

Welcome to the FiveM Lua Library documentation! This library provides a clean and intuitive API for FiveM development using metatables to create shorter, more readable code.

## 🚀 Quick Start

1. **Installation**: Place `fivem-library.lua` in your resource folder
2. **Setup**: Add to your `fxmanifest.lua`
3. **Usage**: Start coding with the simplified API

```lua
local fivem = require('fivem-library')
-- Or use global access (automatically available)
```

## 📚 Documentation Sections

### Core Features
- **[Events](Events.md)** - Simplified event handling
- **[Players](Players.md)** - Player management and manipulation
- **[Vehicles](Vehicles.md)** - Vehicle spawning and control
- **[Utilities](Utilities.md)** - Helper functions and utilities

### Advanced Features
- **[Class System](Class-System.md)** - Object-oriented programming with inheritance
- **[Examples](Examples.md)** - Complete usage examples
- **[API Reference](API-Reference.md)** - Complete method reference

## 🎯 Key Benefits

- **Shorter Code**: `events.add()` instead of `AddEventHandler()`
- **Better Organization**: Namespaced functions for different categories
- **Consistent API**: Similar patterns across all functions
- **Easy to Extend**: Add new methods by extending the metatables
- **Backward Compatible**: Still works with regular FiveM natives
- **OOP Support**: Full class system with inheritance and polymorphism
- **Type Safety**: Instance checking and class identification

## 🔧 Installation

1. Place `fivem-library.lua` in your resource folder
2. Add it to your `fxmanifest.lua`:

```lua
client_scripts {
  'fivem-library.lua',
  'your-script.lua'
}
```

## 📖 Basic Usage Example

```lua
-- Events
fivem.events.add('playerSpawned', function()
  print('Player spawned!')
end)

-- Players
local coords = fivem.players.getCoords()
fivem.players.teleport({x = 100.0, y = 200.0, z = 30.0})

-- Vehicles
local vehicle = fivem.vehicles.spawn('adder', {x = 0.0, y = 0.0, z = 30.0})


```

## 🤝 Contributing

Feel free to extend the library by adding new methods to the appropriate metatables. See the [Extending the Library](Class-System.md#extending-the-library) section for more details.

---

*This documentation is based on the FiveM Lua Library README.md file.* 