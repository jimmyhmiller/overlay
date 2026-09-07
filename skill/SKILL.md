---
name: annotate-screen
description: Ask the user for precise visual feedback by opening Overlay, then inspect the annotated PNG it returns.
---

# Annotate Screen

Use this skill when visual feedback about the current screen would resolve an
ambiguity or when the user asks to mark up the screen.

1. Run `/Users/jimmyhmiller/Documents/Code/projects/overlay/build/release/overlay --capture` and wait for it to exit. Do not choose an output path unless the user explicitly requests one.
2. Overlay creates a unique session under `~/.overlay/`. **Snap** records a numbered image, clears the canvas, and stays open; **Finish** records remaining marks and ends the session.
3. Tell the user the overlay is ready. They can draw boxes, arrows, ink, or text; Return/Finish submits and Escape cancels.
4. On exit status 0, read every path between `OVERLAY_SERIES_BEGIN` and `OVERLAY_SERIES_END`, in order, and inspect every image with the available image-viewing tool.
5. Treat exit status 1 as user cancellation. Treat status 2 as capture failure and point the user to macOS Screen & System Audio Recording permission.
6. Preserve the image until the task is complete so later reasoning can refer back to it.

Never infer annotations from an output file produced by a failed or cancelled run.
