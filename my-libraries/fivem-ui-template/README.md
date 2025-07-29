# FiveM UI Template

A simple, modular template for building in-game UIs in FiveM using HTML, CSS, and JavaScript. The included **UIManager** class lets you create and control UI elements dynamically—no manual HTML editing required.

---

## 🚀 Features

- **Dynamic UI Creation** – Build UIs in JavaScript, not HTML.
- **NUI Callbacks** – Easily send/receive events between your UI and FiveM.
- **Automatic Close Buttons** – Built-in support for closing UIs.
- **Custom Styles** – Add your own CSS for full customization.

---

## 📦 Installation

1. **Clone or Download:**
    ```sh
    git clone https://github.com/your-repo-name/FiveM-UI-Template.git
    ```
2. **Move to Your Resources Folder:**
    ```sh
    mv FiveM-UI-Template your-fivem-server/resources/
    ```
3. **Add to `server.cfg`:**
    ```
    ensure fivem-html-boilerplate
    ```

---

## ⚡ Quick Start

See [Usage.md](./Usage.md) for detailed usage instructions and examples.

---

## 📂 File Structure

```
FiveM-UI-Template/
│── fxmanifest.lua       # Resource manifest
│── html/
│   ├── index.html       # UI entry point
│   ├── custom.css       # Your styles
│   ├── main.js          # Your scripts
│   ├── ui.js            # UIManager class
│── client.lua           # FiveM client events
│── README.md            # Documentation
```

---

For FiveM integration details, see [Integration.md](./Integration.md). 