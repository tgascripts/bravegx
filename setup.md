# BraveGX Setup Guide

Follow the steps below to install and maintain the BraveGX mod on Brave Browser (November 2025 release or newer).

## 1. Verify Brave Version
1. Open **brave://settings/help** in the address bar.
2. Confirm the browser reports **Version 1.75.x (November 2025)** or later.
3. Apply any pending updates and restart Brave before continuing.

## 2. Prepare the Mod Files
1. Clone or download this repository to a convenient location on your computer.
   ```bash
   git clone https://github.com/your-org/BraveGX.git
   ```
2. Extract the archive if you downloaded a `.zip` file.
3. Keep the folder structure intact:
   ```
   BraveGX/
   ├─ modSettings.css
   ├─ setup.md
   ├─ README.md
   ├─ snippets/
   └─ sources/
   ```

## 3. Enable Brave Theme Overrides
1. Open **brave://flags** in Brave.
2. Search for **"Allow CSS UI Styling"** (or equivalent custom styling flag for your platform).
3. Set the flag to **Enabled** and relaunch the browser when prompted.

> **Note:** If the flag is unavailable, ensure you are on Brave desktop (Windows, macOS, or Linux). The mod does not support mobile builds.

## 4. Locate the Brave UI Resources Folder
Brave stores its UI assets in platform-specific locations. Identify the folder that contains the `brave-resources.pak` and `brave_mods` directories:

- **Windows:** `%LOCALAPPDATA%\BraveSoftware\Brave-Browser\Application\<version>\resources\`
- **macOS:** `/Applications/Brave Browser.app/Contents/Frameworks/Brave Browser Framework.framework/Versions/<version>/Resources/`
- **Linux:** `/opt/brave.com/brave/resources/`

Create a backup copy of the original resources folder before proceeding so you can restore the stock UI if needed.

## 5. Install the Mod
1. Inside the resources directory, create (or locate) a `brave_mods` folder.
2. Copy the contents of the `BraveGX` repository into a new subfolder, e.g. `brave_mods/BraveGX/`.
3. Merge the stylesheet references by editing the Brave mod manifest (usually `brave_mods/mods.json`):
   ```json
   {
     "mods": [
       {
         "name": "BraveGX",
         "path": "BraveGX/sources/global.css",
         "snippets": [
           "BraveGX/snippets/hibernated_tabs_indicator.css",
           "BraveGX/snippets/ominibox_rounded_favicons.css"
         ]
       }
     ]
   }
   ```
4. Ensure file paths match your folder structure. Brave must have read access to the files.

## 6. Apply Custom CSS via brave://settings
1. Navigate to **brave://settings/appearance**.
2. Enable **Use custom CSS** (developer mode).
3. Point the custom CSS field to `brave_mods/BraveGX/modSettings.css`.
4. Restart Brave to load the new stylesheets.

## 7. Validate the Installation
1. Open a new window and inspect the address bar, tab strip, sidebar, and status bar for the BraveGX styling.
2. Visit **brave://flags** and confirm your custom styling flag remains enabled.
3. Run through daily workflows (opening tabs, pinning tabs, visiting Speed Dial, using Quick Commands) to ensure all areas adopt the BraveGX appearance.

## 8. Updating the Mod
1. Pull the latest changes from the repository:
   ```bash
   git pull origin main
   ```
2. Replace the contents of your local `brave_mods/BraveGX/` folder with the updated files.
3. Restart Brave to load the changes.

## 9. Troubleshooting
- **Styles did not apply:** Recheck the custom CSS path and ensure the Brave flags are enabled.
- **UI artifacts or glitches:** Clear the `brave_mods` cache by deleting the folder and reinstalling the mod.
- **Brave update reset settings:** After major Brave updates, repeat Steps 3–6 to re-enable custom CSS and copy the mod files again.

## 10. Support
For questions or bug reports, open an issue on the repository or contact **@Zarvanos**. Include screenshots, Brave version, operating system, and steps to reproduce any problems.
