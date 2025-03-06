# Unlocking StarUML: A Step-by-Step Guide

## 1. Install StarUML
Download and install the latest version of [StarUML](http://staruml.io/) from the official website.

## 2. Install Asar
Asar is required to extract and modify StarUML files. Install it globally using npm:
```sh
npm i asar -g
```
**Note:** Ensure you have the LTS version of [Node.js](https://nodejs.org/) installed to avoid compatibility issues.

## 3. Extract `app.asar`
Navigate to the StarUML resources directory:

- **Windows:** `C:/Program Files/StarUML/resources`
- **MacOS:** `/Applications/StarUML.app/Contents/Resources`
- **Linux:** `/opt/staruml/resources`

Use the following command to extract `app.asar`:
```sh
asar e app.asar app
```

## 4. Modify the License Manager
Edit `license-manager.js` located at:
```
Program Files/StarUML/resources/app/src/engine/license-manager.js
```
Replace its content with the modified license manager code provided above.

## 5. Enable High-Resolution Exports (No Watermark)
Modify `diagram-export.js` at:
```
Program Files/StarUML/resources/app/src/diagram-export.js
```
Replace the export logic with the optimized high-resolution export code provided above.

## 6. Repack `app.asar`
Once modifications are complete, repack the `app.asar` file:
```sh
asar pack app app.asar
```

## 7. Clean Up
After repacking, delete the extracted `app` folder:
- **Windows:**
  ```sh
  rmdir /s /q app
  ```
- **Linux/Mac:**
  ```sh
  rm -rf app
  ```

## 8. Launch StarUML
Run `StarUML.exe` from the installation directory or via the desktop shortcut.

---
**Now enjoy a fully unlocked StarUML experience!** 🚀

