# FiveM Integration

## Communicating Between React and Lua

Your React UI can send and receive messages to/from your FiveM Lua scripts using NUI (Native UI) messages.

### Sending a Message from React to Lua

In your React component, use the `fetch` API to send a message:

```js
// Example: Send a message to Lua
fetch('https://lua-x-react-boilerplate/myEvent', {
  method: 'POST',
  body: JSON.stringify({ foo: 'bar' })
});
```

### Receiving a Message in Lua

In your `client.lua`:

```lua
RegisterNUICallback('myEvent', function(data, cb)
  print('Received from React:', data.foo)
  cb('ok')
end)
```

### Sending a Message from Lua to React

In your `client.lua`:

```lua
SendNUIMessage({ action = 'showMessage', text = 'Hello from Lua!' })
```

In your React app, listen for messages:

```js
// Example: Listen for messages from Lua
window.addEventListener('message', (event) => {
  if (event.data.action === 'showMessage') {
    alert(event.data.text);
  }
});
```

***

This is the basic pattern for integrating your React UI with FiveM Lua scripts.
