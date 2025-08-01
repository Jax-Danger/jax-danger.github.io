# Usage (React/UI)

This page covers how to work with the React UI side of the lua-x-react boilerplate.

## Creating a Basic React Component

Let's create a simple React component to display a message.

1. **Create a new file:**
   - Go to `web/src/components/`
   - Create a file named `HelloWorld.tsx` (or `HelloWorld.jsx` if using JavaScript)

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
   - Open `web/src/App.tsx` (or `App.jsx`)
   - Import and add your new component:

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
   - Run the dev server:
     ```sh
     npm run dev
     ```
   - Open the provided local URL in your browser. You should see your new component rendered on the page!

---

## Communicating with the Client (Lua) Side

### Sending Data to the Client

Use the `fetchNui` utility to send data or requests to the client (Lua) side:

```js
import { fetchNui } from 'utils/fetchNui';

fetchNui('myEvent', { foo: 'bar' }).then((response) => {
  console.log('Response from client:', response);
});
```

### Receiving Data from the Client

Use the `useNuiEvent` hook to listen for messages sent from the client (Lua) side:

```js
import { useNuiEvent } from 'utils/useNuiEvent';

useNuiEvent('configData', (data) => {
  // Handle config data sent from the client
  console.log('Received config:', data);
});
```

These utilities are the recommended way to communicate between your React UI and the client scripts.