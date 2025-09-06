# Obsidian Excalidraw Create Linked Canvases

This is a small Obsidian + Excalidraw automation script/plugin. It helps you create **linked Excalidraw canvases** with forward and backward navigation.

## Features

- Insert a ⮩ link at `(0,0)` in the current Excalidraw canvas, pointing to a new file.
- Automatically create a new file named by timestamp (`YYYYMMDDHHmmss.excalidraw.md`).
- Open the new file in the current pane and switch to Excalidraw view.
- Insert a ⮪ link at the center of the new canvas, pointing back to the old file.
- Uses **uncompressed JSON** in the template for clarity and easier customization.

## Demo Workflow

<video src="20250906031544excalidraw - 2025 - Obsidian v1912 2025-09-06 12-35-16_compressed.mp4" controls width="600"></video>

1. Open any `.excalidraw.md` file in Excalidraw view.  
2. Run the command (via the script).  
3. A ⮩ marker appears at `(0,0)` linking to the new file.  
4. A new canvas is created, opened, and switched to Excalidraw view.  
5. The new canvas contains a ⮪ marker in the center, linking back to the original file.

This makes it easy to create a **chain of linked drawings** (like pages or slides).

## Installation

1. Ensure you have installed:
   - [Obsidian](https://obsidian.md)
   - [Excalidraw Plugin](https://github.com/zsviczian/obsidian-excalidraw-plugin)
   - (Optional) [Excalidraw Automate](https://github.com/zsviczian/obsidian-excalidraw-plugin/wiki/Automate) enabled.
2. Copy the script into your vault (for example into a `scripts` folder).
3. Run the script inside Obsidian (with Templater, QuickAdd, or a custom command).

## Example

- Original canvas:  
  ⮩ at `(0,0)` → points to `20250906120000.excalidraw.md`  

- New canvas:  
  ⮪ in the center → points back to the original file.

## License

MIT License. Feel free to use and adapt.

---
