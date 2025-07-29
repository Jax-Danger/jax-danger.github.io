# TypeScript Starter Pack for FiveM

A comprehensive TypeScript boilerplate for FiveM development with modern tooling, type safety, and best practices.

## 🚀 Quick Start

### Prerequisites
- Node.js (v16 or higher)
- Yarn package manager
- FiveM server

### Installation

1. Clone or download this starter pack
2. Navigate to the project directory
3. Install dependencies:
   ```bash
   yarn
   ```

### Development Commands

- `yarn build` - Build TypeScript files to JavaScript
- `yarn dev` - Build and obfuscate files for production
- `yarn watch` - Auto-build TypeScript files (development mode)

## 📁 Project Structure

```
ts-starterpack/
├── src/
│   ├── client/          # Client-side scripts
│   ├── server/          # Server-side scripts
│   └── shared/          # Shared types and utilities
├── types/
│   └── global.d.ts      # Global type definitions
├── dist/                # Compiled JavaScript files
└── package.json
```

## 🔧 TypeScript Configuration

### Type References

**IMPORTANT**: Add the appropriate type reference at the top of every file:

**For client-side files:**
```typescript
/// <reference types="@citizenfx/client" />
```

**For server-side files:**
```typescript
/// <reference types="@citizenfx/server" />
```

This enables IntelliSense and type checking for FiveM native functions.

## 📚 Documentation

- Getting Started - Detailed setup and first steps
- TypeScript Types - TypeScript type system and annotations
- TypeScript Basics - TypeScript fundamentals for FiveM
- Client-Side Development - Client-side scripting examples
- Server-Side Development - Server-side scripting examples
- Event System - Working with FiveM events
- Database Integration - Database operations with TypeScript
- UI Development - Creating user interfaces
- Best Practices - Coding standards and patterns

## 🎯 Key Features

- **Type Safety**: Full TypeScript support with FiveM native types
- **Modern Tooling**: Yarn, ESLint, and Prettier configuration
- **Hot Reload**: Automatic compilation during development
- **Production Ready**: Obfuscation and optimization for deployment
- **Modular Structure**: Organized codebase with clear separation of concerns

## 🔗 Global Types

All global variables, interfaces, and types are defined in `types/global.d.ts`. By default, it includes:

- Configuration interfaces
- Custom type definitions
- Global utility types

You can extend this file to add your own global types and interfaces.

## 🤝 Contributing

Feel free to submit issues, feature requests, or pull requests to improve this starter pack.

## 📄 License

This project is open source and available under the MIT License.
