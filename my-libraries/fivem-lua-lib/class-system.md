# Class System

The Class System provides a simple and intuitive way to create object-oriented code in Lua, making it easier to organize and structure your FiveM scripts.

### Compatibility

✅ **Shared** - Works on both client and server-side

### Basic Usage

```lua
-- Create a class
local Animal = fivem.Class.create("Animal")

-- Or use global access (no fivem prefix needed)
local Animal = Class.create("Animal")

-- Add constructor
Animal:constructor(function(self, name)
  self.name = name
end)

-- Add methods
Animal:method("getName", function(self)
  return self.name
end)

-- Create instance
local animal = Animal:new("Generic Animal")

-- Or use global access
local animal = Animal:new("Generic Animal")

-- Use methods
print(animal:getName()) -- Output: Generic Animal
```

### Available Methods

#### `Class.create(name)`

Creates a new class with the specified name.

**Parameters:**

* `name` (string) - The name of the class

**Returns:**

* `table` - The created class

**Example:**

```lua
local Animal = fivem.Class.create("Animal")

-- Or use global access
local Animal = Class.create("Animal")
```

#### `Class:constructor(func)`

Defines the constructor function for the class.

**Parameters:**

* `func` (function) - The constructor function

**Example:**

```lua
local Animal = fivem.Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

-- Or use global access
local Animal = Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)
```

#### `Class:method(name, func)`

Adds a method to the class.

**Parameters:**

* `name` (string) - The name of the method
* `func` (function) - The method function

**Example:**

```lua
local Animal = fivem.Class.create("Animal")

Animal:method("getName", function(self)
  return self.name
end)

Animal:method("makeSound", function(self)
  return "Some animal sound"
end)

-- Or use global access
local Animal = Class.create("Animal")

Animal:method("getName", function(self)
  return self.name
end)

Animal:method("makeSound", function(self)
  return "Some animal sound"
end)
```

#### `Class:static(name, func)`

Adds a static method to the class.

**Parameters:**

* `name` (string) - The name of the static method
* `func` (function) - The static method function

**Example:**

```lua
local Animal = fivem.Class.create("Animal")

Animal:static("createDefault", function(name)
  return Animal:new(name)
end)

-- Or use global access
local Animal = Class.create("Animal")

Animal:static("createDefault", function(name)
  return Animal:new(name)
end)

-- Usage
local animal = Animal.createDefault("Generic Animal")
```

#### `Class:new(...)`

Creates a new instance of the class.

**Parameters:**

* `...` - Arguments to pass to the constructor

**Returns:**

* `table` - The new instance

**Example:**

```lua
local Animal = fivem.Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local animal = Animal:new("Generic Animal")

-- Or use global access
local Animal = Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local animal = Animal:new("Generic Animal")
```

#### `Class:extend(name)`

Creates a new class that extends the current class.

**Parameters:**

* `name` (string) - The name of the new class

**Returns:**

* `table` - The new extended class

**Example:**

```lua
local Animal = fivem.Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)

-- Or use global access
local Animal = Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)
```

#### `Class:private(name, func)`

Adds a private method to the class (convention-based).

**Parameters:**

* `name` (string) - The name of the private method
* `func` (function) - The private method function

**Example:**

```lua
local Animal = fivem.Class.create("Animal")

Animal:private("validateName", function(self, name)
  return name and type(name) == 'string'
end)

Animal:method("setName", function(self, name)
  if self:validateName(name) then
    self.name = name
  end
end)

-- Or use global access
local Animal = Class.create("Animal")

Animal:private("validateName", function(self, name)
  return name and type(name) == 'string'
end)

Animal:method("setName", function(self, name)
  if self:validateName(name) then
    self.name = name
  end
end)
```

#### `instance:super(...)`

Calls the parent class constructor or method.

**Parameters:**

* `...` - Arguments to pass to the parent

**Example:**

```lua
local Animal = fivem.Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)

-- Or use global access
local Animal = Class.create("Animal")
Animal:constructor(function(self, name)
  self.name = name
end)

local Dog = Animal:extend("Dog")
Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)
```

### Inheritance Example

```lua
-- Extended class
local Dog = Animal:extend("Dog")

Dog:constructor(function(self, name, breed)
  self:super(name) -- Call parent constructor
  self.breed = breed
end)

Dog:method("makeSound", function(self)
  return "Woof!"
end)

Dog:method("getBreed", function(self)
  return self.breed
end)

-- Usage
local dog = Dog:new("Buddy", "Golden Retriever")
print(dog:getName()) -- Output: Buddy (inherited from Animal)
print(dog:makeSound()) -- Output: Woof! (overridden in Dog)
print(dog:getBreed()) -- Output: Golden Retriever (Dog method)
```

### Best Practices

1. **Use descriptive class names** - Make class names clear and specific
2. **Initialize all properties** - Set default values in the constructor
3. **Validate inputs** - Always validate parameters in methods
4. **Use inheritance appropriately** - Don't over-engineer simple classes
