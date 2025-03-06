# Unlocking StarUML: A Step-by-Step Guide

### 1. Install StarUML
Download the latest version of StarUML from the official website.

### 2. Install Asar
Next, install `asar`, a utility to manage `.asar` files. Open your terminal as an administrator and run the following command:

```sh
npm i asar -g
```

#### Important
Make sure to have the **LTS version of Node.js** installed to ensure compatibility and avoid errors when running npm commands. You can download the LTS version from [nodejs.org](https://nodejs.org/).

### 3. Extract `app.asar`
To access the files needed to modify the license and export settings, extract the `app.asar` file.

Navigate to the StarUML directory. By default, it’s located at:

- **Windows:** `C:/Program Files/StarUML/resources`
- **macOS:** `/Applications/StarUML.app/Contents/Resources`
- **Linux:** `/opt/staruml/resources`

Use the `cd` command to navigate to your specific directory. For example:

```sh
cd "C:/Program Files/StarUML/resources"
```

Run the following command in your terminal as an **Administrator** (Git Bash, PowerShell, or CMD):

```sh
asar e app.asar app
```

This will extract the `app.asar` file into a folder called `app`.

---

### 4. Modify the License Manager
In the extracted files, navigate to the following path:

```sh
Program Files/StarUML/resources/app/src/engine/license-manager.js
```

Open the `license-manager.js` file in your preferred code editor and replace its contents with the following modified license manager code:

```javascript
const { EventEmitter } = require("events");
const fs = require("fs");
const path = require("path");
const crypto = require("crypto");
const UnregisteredDialog = require("../dialogs/unregistered-dialog");
const packageJSON = require("../../package.json");

const SK = "DF9B72CC966FBE3A46F99858C5AEE";
const PRO_DIAGRAM_TYPES = [
    "SysMLRequirementDiagram",
    "SysMLBlockDefinitionDiagram",
    "SysMLInternalBlockDiagram",
    "SysMLParametricDiagram",
    "BPMNDiagram",
    "WFWireframeDiagram",
    "AWSDiagram",
    "GCPDiagram",
];

var status = true; // Always set to true
var licenseInfo = null;

class LicenseManager extends EventEmitter {
    constructor() {
        super();
        this.projectManager = null;
    }

    isProDiagram(diagramType) {
        return PRO_DIAGRAM_TYPES.includes(diagramType);
    }

    getStatus() {
        return true;
    }

    getLicenseInfo() {
        return { valid: true };
    }

    async checkLicenseValidity() {
        status = true;
    }

    appReady() {
        this.checkLicenseValidity();
    }
}

module.exports = LicenseManager;
```

This modification ensures that StarUML always operates in a registered state.

---

### 5. Exporting Diagrams in High Resolution (Without Watermarks)
To export diagrams in high resolution without watermarks, locate and modify the following file:

```sh
Program Files/StarUML/resources/app/src/diagram-export.js
```

Open the `diagram-export.js` file and replace its contents with the following code:

```javascript
const fs = require("fs-extra");
const filenamify = require("filenamify");
const { Point, ZoomFactor, Canvas } = require("../core/graphics");
const { Context } = require("svgcanvas");

const BOUNDING_BOX_EXPAND = 10;

function getImageData(diagram, type) {
    var canvasElement = document.createElement("canvas");
    var canvas = new Canvas(canvasElement.getContext("2d"));
    var boundingBox = diagram.getBoundingBox(canvas);
    boundingBox.expand(BOUNDING_BOX_EXPAND);
    canvas.origin = new Point(-boundingBox.x1, -boundingBox.y1);
    canvas.zoomFactor = new ZoomFactor(1, 1);
    canvasElement.width = boundingBox.getWidth();
    canvasElement.height = boundingBox.getHeight();

    if (window.devicePixelRatio) {
        var ratio = window.devicePixelRatio * 2;
        canvasElement.width *= ratio;
        canvasElement.height *= ratio;
        canvas.context.scale(ratio, ratio);
    }

    if (type === "image/jpeg") {
        canvas.context.fillStyle = "#ffffff";
        canvas.context.fillRect(0, 0, canvasElement.width, canvasElement.height);
    }

    diagram.arrangeDiagram(canvas);
    diagram.drawDiagram(canvas);
    return canvasElement.toDataURL(type, 1.0).replace(/^data:image\/(png|jpeg);base64,/, "");
}

function exportToPNG(diagram, fullPath) {
    diagram.deselectAll();
    var data = getImageData(diagram, "image/png");
    var buffer = Buffer.from(data, "base64");
    fs.writeFileSync(fullPath, buffer);
}

function exportToJPEG(diagram, fullPath) {
    diagram.deselectAll();
    var data = getImageData(diagram, "image/jpeg");
    var buffer = Buffer.from(data, "base64");
    fs.writeFileSync(fullPath, buffer);
}

exports.exportToPNG = exportToPNG;
exports.exportToJPEG = exportToJPEG;
```

This modification ensures high-resolution exports without watermarks.

---

### 6. Repack `app.asar`
Once you have edited the necessary files, you need to repack the `app.asar` file. Navigate back to the `resources` directory and run the following command:

```sh
asar pack app app.asar
```

This will repack your modified `app` folder back into a `.asar` file.

---

### 7. Clean Up
After repacking the `app.asar`, you can safely remove the extracted `app` folder to clean up your directory.

For Windows:
```sh
rmdir /s /q app
```
For Linux or macOS:
```sh
rm -rf app
```

---

### 8. Launch StarUML
Now that everything is set up, launch StarUML by running the `StarUML.exe` file from your installation directory or through the desktop shortcut.


**Now enjoy a fully unlocked StarUML experience!** 🚀

