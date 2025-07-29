# Getting Started with TypeScript for FiveM

This guide will walk you through setting up and using the TypeScript starter pack for FiveM development.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v16 or higher) - [Download here](https://nodejs.org/)
- **Yarn** package manager - Install via `npm install -g yarn`
- **FiveM Server** - [Download here](https://runtime.fivem.net/artifacts/fivem/build_server_windows/master/)
- **Code Editor** - VS Code recommended with TypeScript extensions

## Installation

1. **Clone or download the starter pack**
   ```bash
   git clone <repository-url>
   cd ts-starterpack
   ```

2. **Install dependencies**
   ```bash
   yarn
   ```

3. **Verify installation**
   ```bash
   yarn build
   ```

## Project Structure

```
ts-starterpack/
├── src/
│   ├── client/          # Client-side scripts
│   │   ├── main.ts      # Main client entry point
│   │   └── ui/          # UI components
│   ├── server/          # Server-side scripts
│   │   ├── main.ts      # Main server entry point
│   │   └── database/    # Database operations
│   └── shared/          # Shared types and utilities
│       ├── types.ts     # Common type definitions
│       └── utils.ts     # Utility functions
├── types/
│   └── global.d.ts      # Global type definitions
├── dist/                # Compiled JavaScript files
├── package.json         # Project dependencies
├── tsconfig.json        # TypeScript configuration
└── fxmanifest.lua       # FiveM resource manifest
```

## First Steps

### 1. Understanding Type References

Every TypeScript file in FiveM needs a type reference at the top:

**Client-side file (`src/client/main.ts`):**
```typescript
/// <reference types="@citizenfx/client" />

// Your client-side code here
```

**Server-side file (`src/server/main.ts`):**
```typescript
/// <reference types="@citizenfx/server" />

// Your server-side code here
```

### 2. Creating Your First Script

Let's create a simple "Hello World" script:

**Client-side (`src/client/hello.ts`):**
```typescript
/// <reference types="@citizenfx/client" />

// Display a notification when the resource starts
AddEventHandler('onClientResourceStart', (resourceName: string) => {
    if (GetCurrentResourceName() !== resourceName) return;
    
    SetNotificationTextEntry('STRING');
    AddTextComponentString('Hello from TypeScript!');
    DrawNotification(false, false);
});
```

**Server-side (`src/server/hello.ts`):**
```typescript
/// <reference types="@citizenfx/server" />

// Log a message when the resource starts
AddEventHandler('onResourceStart', (resourceName: string) => {
    if (GetCurrentResourceName() !== resourceName) return;
    
    console.log('TypeScript resource started successfully!');
});
```

### 3. Importing Your Scripts (CRITICAL)

**⚠️ IMPORTANT**: You MUST import all your scripts into the main entry files, or they won't be built!

**Client-side (`src/client/main.ts`):**
```typescript
/// <reference types="@citizenfx/client" />

// Import all your client-side scripts here
import './hello';
// import './ui/interface';
// import './vehicles/manager';
// import './players/controller';
```

**Server-side (`src/server/main.ts`):**
```typescript
/// <reference types="@citizenfx/server" />

// Import all your server-side scripts here
import './hello';
// import './database/connection';
// import './players/manager';
// import './vehicles/spawner';
```

**Why this is important:**
- The TypeScript compiler only builds files that are imported
- If you create a new file but don't import it, it won't be included in the build
- Always add your new scripts to the appropriate main.ts file

### 4. Building and Testing

1. **Build the project:**
   ```bash
   yarn build
   ```

2. **Copy the `dist` folder to your FiveM server resources directory**

3. **Add to your server.cfg:**
   ```
   ensure your-resource-name
   ```

4. **Start your FiveM server and test the resource**

## Development Workflow

### Development Mode
```bash
yarn watch
```
This command watches for file changes and automatically rebuilds your TypeScript files.

### Production Build
```bash
yarn dev
```
This command builds and obfuscates your code for production deployment.

### Manual Build
```bash
yarn build
```
This command simply compiles TypeScript to JavaScript without obfuscation.

## Configuration Files

### TypeScript Configuration (`tsconfig.json`)
The starter pack includes a pre-configured TypeScript configuration optimized for FiveM development.

### FiveM Manifest (`fxmanifest.lua`)
The manifest file is automatically generated from your TypeScript source files.

## Common Issues and Solutions

### Issue: Native functions not recognized
**Solution:** Ensure you have the correct type reference at the top of your file.

### Issue: Build errors
**Solution:** Check that all dependencies are installed with `yarn install`.

### Issue: Resource not loading
**Solution:** Verify the `dist` folder is in your server's resources directory and the resource is properly configured in `server.cfg`.


## Getting Help

If you encounter issues:

1. Check the [FiveM documentation](https://docs.fivem.net/)
2. Review the [TypeScript documentation](https://www.typescriptlang.org/docs/)
3. Search existing issues or create a new one in the repository 