---
description: >-
  This page covers how to work with the client (Lua) side of the lua-x-react
  boilerplate.
---

# FiveM Integration (Client/Lua)

### Sending Data to the UI

Use the included `SendReactMessage` helper to send data from your client script to the React UI:

```lua
RegisterCommand("ui", function()
  SetNuiFocus(true, true) -- allows mouse and keyboard input in the UI

  SendReactMessage('configData', {
    ServerName    = cfg.ServerName,
    MaxPlayers    = cfg.MaxPlayers,
    StartingMoney = cfg.StartingMoney,
    isPvpEnabled  = cfg.EnablePvP
  })
end)
```

> **Note:** This boilerplate is set up to use `type` in `SendNUIMessage` (not `action`) to avoid confusion and for consistency in your React event handlers.

You can also use `SendNUIMessage` directly if you need more control:

```lua
SendNUIMessage({ type = 'showMessage', text = 'Hello from Lua!' })
```

### Receiving Data from the UI

To handle data sent from the React UI, use `RegisterNUICallback`:

```lua
RegisterNUICallback('myEvent', function(data, cb)
  print('Received from UI:', data.foo)
  cb('ok')
end)
```

### Setting NUI Focus

To allow the user to interact with the UI (mouse/keyboard), use:

```lua
SetNuiFocus(true, true)
```

Set it to `false, false` to remove focus when closing the UI.

***

These are the recommended ways to integrate your client scripts with the React UI in this boilerplate.
