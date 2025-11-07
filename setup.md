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

> **Note:** If the flag is unavailable, ensure you are on Brave desktop (Windows, macOS, or Linux). The mod does not support mobile builds. Workarounds for missing options are documented in [Section 11](#11-workarounds-when-customization-options-are-missing).

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

> **Tip:** If the toggle is missing, consult [Section 11](#11-workarounds-when-customization-options-are-missing) for alternate activation methods.

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

## 11. Workarounds When Customization Options Are Missing

If Brave removes or hides the UI controls referenced above, use one of the following alternatives to keep the BraveGX mod active:

### Option A: Load the Mod Exclusively Through `mods.json`
1. Open the `brave_mods/mods.json` file you edited in [Step 5](#5-install-the-mod).
2. Add `modSettings.css` as an additional entry in the `snippets` array so Brave loads every stylesheet from the manifest:
   ```json
   {
     "mods": [
       {
         "name": "BraveGX",
         "path": "BraveGX/sources/global.css",
         "snippets": [
           "BraveGX/snippets/hibernated_tabs_indicator.css",
           "BraveGX/snippets/ominibox_rounded_favicons.css",
           "BraveGX/modSettings.css"
         ]
       }
     ]
   }
   ```
3. Save the file and relaunch Brave. The mod framework will now import every stylesheet without requiring the **Use custom CSS** toggle.

### Option B: Force a Custom CSS Path via Preferences
1. Close Brave completely so it saves profile settings.
2. Open your Brave profile directory:
   - Windows: `%LOCALAPPDATA%\BraveSoftware\Brave-Browser\User Data\Default\`
   - macOS: `~/Library/Application Support/BraveSoftware/Brave-Browser/Default/`
   - Linux: `~/.config/BraveSoftware/Brave-Browser/Default/`
3. Edit the `Preferences` file (JSON) and add or update the following block near the top level:
   ```json
   "brave": {
     "appearance": {
       "custom_css_location": "C:/path/to/brave_mods/BraveGX/modSettings.css"
     }
   }
   ```
   > Use platform-appropriate paths (e.g., `/Users/<name>/...` on macOS and `/home/<name>/...` on Linux).
4. Save the file and restart Brave. The browser reads this preference on launch even when the UI toggle is hidden.

### Option C: Bundle the Mod as a Local Extension
1. Create a new folder, e.g., `BraveGX-extension/`, containing a `manifest.json` with:
   ```json
   {
     "manifest_version": 3,
     "name": "BraveGX Styles",
     "version": "1.0.0",
     "content_scripts": [
       {
         "matches": ["<all_urls>"],
         "css": ["modSettings.css"],
         "run_at": "document_start"
       }
     ]
   }
   ```
2. Copy `modSettings.css` (and any additional files you need) into the extension folder.
3. Open **brave://extensions**, enable **Developer mode**, and click **Load unpacked**.
4. Select the `BraveGX-extension/` folder. Brave will inject the CSS at runtime even without native UI hooks.

Choose the option that best fits your workflow. Revert the changes if Brave reintroduces the missing controls to avoid maintaining multiple override paths.
