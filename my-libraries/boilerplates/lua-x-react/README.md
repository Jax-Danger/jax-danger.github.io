# Lua x React Boilerplate

A boilerplate for building FiveM UIs using React (TypeScript or JavaScript) and Lua. This template helps you quickly set up a modern UI for your FiveM scripts, with easy integration between Lua and React.

---

## 🚀 Features

- **React Frontend** – Build UIs with React (choose TypeScript or JavaScript).
- **Lua Integration** – Communicate between your React UI and FiveM Lua scripts.
- **SCSS Support** – Style your UI with SCSS for advanced styling.
- **Hot Reload/Dev Server** – Fast development with live reloading.

---

## 📦 Installation

1. **Download the Boilerplate:**
   - Visit the [GitHub releases page](https://github.com/Jax-Danger/lua-react-boilerplate/releases).
   - Choose your preferred version: **TypeScript** or **JavaScript**.
   - Download and extract to your FiveM resources folder.
2. **Add to `server.cfg`:**
   ```
   ensure lua-react-boilerplate
   ```

---

## ⚡ Quick Start

1. **Install dependencies:**
   ```sh
   cd web
   npm install
   ```
2. **Build or run in dev mode:**
   - Build for production:
     ```sh
     npm run build
     ```
   - Or run the dev server for hot reload:
     ```sh
     npm run dev
     ```
3. **Create your own components:**
   - Go to `web/src/components` and add your new component files.
   - Import and use them in `App.tsx` (or `App.jsx`) in `web/src` to make them visible in your UI.

---

## 📂 File Structure

```
lua-x-react-boilerplate/
│── fxmanifest.lua       # Resource manifest
│── client/              # Lua client scripts
│── server/              # Lua server scripts
│── web/
│   ├── src/
│   │   ├── components/  # Your React components
│   │   ├── App.tsx      # Main React app (edit to add your components)
│   ├── ...
│── README.md            # Documentation
```