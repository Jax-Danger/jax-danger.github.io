# Getting Started with FiveM Lua Library

## 🚀 Welcome to the FiveM Lua Library!

Before diving into the documentation, let's make sure you're ready to use this library effectively. This guide will help you understand what this library is, what you need to know, and how to get started.

## 📋 Prerequisites

### What You Should Know:
- **Basic Lua programming** (variables, functions, tables)
- **FiveM development fundamentals** (events, natives, client/server scripts)
- **How to create and manage FiveM resources**

### What You DON'T Need to Know:
- **Advanced metatable programming** (the library handles this for you)
- **Complex FiveM API patterns** (we simplify them)
- **Object-oriented programming** (though it's available if you want it)

## 🎯 What This Library Does

The FiveM Lua Library is designed to make FiveM development **faster, cleaner, and more readable**. It does this by:

### 1. **Simplifying Common Tasks**
Instead of writing:
```lua
AddEventHandler('playerSpawned', function()
    local playerPed = GetPlayerPed(-1)
    local coords = GetEntityCoords(playerPed)
    SetEntityCoords(playerPed, coords.x + 10.0, coords.y, coords.z)
end)
```

You can write:
```lua
fivem.events.add('playerSpawned', function()
    local coords = fivem.players.getCoords()
    fivem.players.teleport({x = coords.x + 10.0, y = coords.y, z = coords.z})
end)
```

### 2. **Providing Consistent APIs**
All functions follow similar patterns, making them easier to remember and use.

### 3. **Adding Object-Oriented Features**
Create classes, inheritance, and organized code structures.

## 🛠️ Installation

### Step 1: Download the Library
Place `fivem-library.lua` in your resource folder.

### Step 2: Add to Your Manifest
```lua
-- fxmanifest.lua
fx_version 'cerulean'
game 'gta5'

client_scripts {
    'fivem-library.lua',
    'your-script.lua'
}
```

### Step 3: Start Using It
```lua
-- your-script.lua
local fivem = require('fivem-library')
-- Or use global access (automatically available)
```

## 📚 How to Navigate the Documentation

### Start Here (Recommended Order):
1. **[Welcome](welcome.html)** - Meet the creator
2. **[Getting Started](getting-started.html)** - You are here! 👈
3. **[Events](Events.html)** - Learn event handling
4. **[Players](Players.html)** - Player management
5. **[Vehicles](Vehicles.html)** - Vehicle operations
6. **[Utilities](Utilities.html)** - Helper functions
7. **[Class System](Class-System.html)** - Object-oriented features
8. **[Examples](Examples.html)** - Complete examples
9. **[API Reference](API-Reference.html)** - Complete method list

### For Different Experience Levels:

**🟢 Beginner (New to FiveM):**
- Start with Events → Players → Vehicles
- Skip Class System for now
- Focus on basic examples

**🟡 Intermediate (Some FiveM experience):**
- Read all sections in order
- Try the Class System
- Experiment with Examples

**🔴 Advanced (Experienced FiveM developer):**
- Jump to API Reference for quick lookup
- Explore Class System for OOP features
- Check Examples for advanced patterns

## 💡 Quick Tips for Success

### 1. **Start Small**
Don't try to rewrite your entire script at once. Start with one feature:
```lua
-- Start with just events
fivem.events.add('playerSpawned', function()
    print('Player spawned!')
end)
```

### 2. **Use the Global Access**
You don't need to require the library - it's available globally:
```lua
-- This works automatically
events.add('playerSpawned', function()
    print('Player spawned!')
end)
```

### 3. **Check the Examples**
The [Examples](Examples.html) section shows complete, working code that you can copy and modify.

### 4. **Use the API Reference**
Keep the [API Reference](API-Reference.html) open for quick method lookups.

## 🔧 Common Patterns You'll See

### Event Handling
```lua
fivem.events.add('eventName', function(data)
    -- Your code here
end)
```

### Player Operations
```lua
local coords = fivem.players.getCoords()
fivem.players.teleport({x = 100.0, y = 200.0, z = 30.0})
```

### Vehicle Operations
```lua
local vehicle = fivem.vehicles.spawn('adder', {x = 0.0, y = 0.0, z = 30.0})
```

### Utility Functions
```lua
fivem.utils.wait(1000) -- Wait 1 second
fivem.utils.print('Hello World')
```

## ❓ Getting Help

### If Something Doesn't Work:
1. **Check the Examples** - Look for similar code
2. **Check the API Reference** - Verify method names and parameters
3. **Try the traditional FiveM way** - The library is backward compatible
4. **Check the browser console** - For any JavaScript errors (if using UI features)

### Still Stuck?
- Check the [Examples](Examples.html) section for complete working code
- Look at the [API Reference](API-Reference.html) for method details
- The library is designed to be intuitive - if something feels wrong, it probably is!

## 🎉 You're Ready!

You now have everything you need to start using the FiveM Lua Library effectively. The documentation is designed to be comprehensive but not overwhelming - take it at your own pace!

**Next up:** [Events](Events.html) - Learn how to handle events the easy way!

---

*Happy coding! 🚀* 