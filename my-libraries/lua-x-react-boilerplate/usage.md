# Usage

## Creating a Basic React Component

Let's create a simple React component to display a message.

1. **Create a new file:**
   * Go to `web/src/components/`
   * Create a file named `HelloWorld.tsx` (or `HelloWorld.jsx` if using JavaScript)
2. **Add the following code:**

```tsx
// web/src/components/HelloWorld.tsx
import React from 'react';

const HelloWorld: React.FC = () => {
  return <h2>Hello from your new component!</h2>;
};

export default HelloWorld;
```

3. **Use your component in the main app:**
   * Open `web/src/App.tsx` (or `App.jsx`)
   * Import and add your new component:

```tsx
// web/src/App.tsx
import React from 'react';
import HelloWorld from './components/HelloWorld';

function App() {
  return (
    <div>
      <h1>Welcome to Lua x React Boilerplate</h1>
      <HelloWorld />
    </div>
  );
}

export default App;
```

4. **See your changes:**
   *   Run the dev server:

       ```sh
       npm run dev
       ```
   * Open the provided local URL in your browser. You should see your new component rendered on the page!
