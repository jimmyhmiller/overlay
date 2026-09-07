# Overlay

Overlay is a menu-bar-only macOS annotation surface written in Coil. It draws
directly over the current screen and exports boxes, arrows, freehand marks, and
editable text as an ordered PNG series for an agent to inspect.

## Build and run

```sh
coil verify
coil build
coil run tools/bundle.coil
open build/Overlay.app
```

The app appears as `✎` in the menu bar and intentionally has no Dock icon.
Choose **Annotate Screen**, mark the screen, then press **Snap** to keep taking
images or **Finish** to return the series. Return also finishes.
Escape cancels. Command-Z undoes the latest drawn gesture.

The palette lives vertically along the right edge, safely below the menu-bar
and notch region. Choose the pointer tool (or press V) to make the annotation
surface click-through while the palette remains available, allowing you to
operate the underlying app between screenshots. Use R for rectangle, A for
arrow, P for pen, T for text, S for Snap, and Return for Finish.

Pointer is selected when each session begins, leaving the underlying app
interactive until you choose an annotation tool. Undo and Redo operate on one
ordered history containing complete drawing gestures and text objects; use
Command-Z and Shift-Command-Z or the palette controls.

The color well in the palette opens the native macOS color panel. The selected
color applies to newly created rectangles, arrows, pen strokes, and text;
existing annotations retain the color they were created with.

Text commits when you press Return or choose another palette command, then
automatically returns to Pointer mode. Committed text remains independently
clickable while the rest of the canvas is click-through: click once to select
it and press Delete to remove it; double-click to edit it again. Text creation
and deletion both participate in Undo and Redo.

Use the Move tool (or press M) to reposition existing annotations. Dragging a
rectangle or arrow moves that object; dragging any part of a pen stroke moves
the complete stroke; dragging text moves its text panel. Every move is a single
Undo/Redo operation. Pointer remains exclusively for interacting with the app
underneath.

For an agent or script, start a capture session without choosing a path. Each
**Snap** writes a numbered image, clears the annotations, and keeps the overlay
open. **Finish** captures any remaining marks and prints the complete ordered
series between `OVERLAY_SERIES_BEGIN` and `OVERLAY_SERIES_END`:

```sh
build/release/overlay --capture
```

Running `build/release/overlay` directly behaves the same way: it is a
one-shot terminal session with no menu-bar item and exits when you Finish or
cancel. Launching `Overlay.app` is the persistent menu-bar form.

Sessions are stored under `~/.overlay/<timestamp>-<pid>-<series>/`. To choose the
session directory explicitly, add `--output /path/to/directory`.

macOS asks for Screen Recording permission on first export. Enable Overlay (or
the terminal used to launch it) in System Settings → Privacy & Security →
Screen & System Audio Recording, then invoke it again.

## Agent skill

The repository includes [`skill/SKILL.md`](skill/SKILL.md). Install the project
and copy or symlink `skill/` into the agent's skills directory.
