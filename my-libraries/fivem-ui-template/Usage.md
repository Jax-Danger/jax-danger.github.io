---
description: This demonstrates how the user would create their own UI solely in javascript.
---

# Javascript Usage(Frontend)

## Creating a UI

In `main.js`:

```javascript
import { UIManager } from "./ui.js";
const uiManager = new UIManager();

uiManager.createUI("myCustomUI", () => `
    <h1>My Custom UI</h1>
    <p>Welcome to my UI!</p>
    <button id="closeBtn" class="close-btn">Close</button>
`);
```

This creates a UI with the ID `myCustomUI`. Show or hide it with `showUI(id)` and `hideUI(id)`.

***

## Show & Hide the UI

**Show:**

```javascript
uiManager.showUI("myCustomUI");
```

**Hide:**

```javascript
uiManager.hideUI("myCustomUI");
```

***

## Add a Close Button

To make a button close your UI:

```javascript
createButton("closeBtn", "closeUI", {}, () => {
    uiManager.hideUI("myCustomUI");
});
```

***

## Customizing with CSS

Edit `custom.css` to change the look:

```css
.ui {
    background: rgba(0, 0, 0, 0.9);
    color: white;
    padding: 20px;
    border-radius: 10px;
}
```

To load multiple stylesheets:

```javascript
uiManager.loadCustomStyles(["custom.css", "dark-theme.css"]);
```

***

For FiveM integration, see Integration.md.
