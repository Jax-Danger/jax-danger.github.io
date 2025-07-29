# FiveM Integration

## Register the UI in `client.lua`

Add a command to open your UI:

```lua
RegisterCommand('openui', function()
    SendNuiMessage(json.encode({ action = 'setDisplay' }))
    SetNuiFocus(true, true)
end)
```

## Handle UI Close in `client.lua`

```lua
RegisterNUICallback('closeUI', function(data, cb)
    SetNuiFocus(false, false)
    SendNUIMessage({ action = "hideUI" })
    cb(json.encode({ success = true }))
end)
```

Now, clicking the close button will hide the UI and remove focus. 