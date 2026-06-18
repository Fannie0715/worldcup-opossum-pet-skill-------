# Output Contract

## Required Pet Package

This package must be compatible with `$hatch-pet`. Treat `$hatch-pet` as the authority for Codex pet atlas geometry, row order, validation, contact-sheet QA, and installation location.

A completed pet package should contain:

```text
<pet-id>/
  pet.json
  spritesheet-daily-board.png
  daily-board-meta.json
```

Optional but recommended:

```text
  spritesheet-daily-board.webp
  validation-daily-board.json
  current-state-map.png
  pet.config.json
```

## Pet JSON Requirements

`pet.json` should include:

- `id`
- `displayName`
- `description`
- `spritesheetPath`
- `display.preferredScale`
- `display.cellWidth`
- `display.cellHeight`
- `stateCopy`
- `embeddedBoard` when schedule is enabled

## Atlas Requirements

The Codex pet atlas is the real product. It must be dynamic, not a contact sheet or one-frame mockup.

The atlas must follow the `$hatch-pet` / Codex contract:

- 8 columns
- 9 rows
- 192 x 208 cell size
- 1536 x 1872 total atlas size
- transparent RGBA background

Rows:

1. `idle`
2. `running-right`
3. `running-left`
4. `waving`
5. `jumping`
6. `failed`
7. `waiting`
8. `running`
9. `review`

Recommended frame counts:

- `idle`: 6 frames
- `running-right`: 8 frames
- `running-left`: 8 frames
- `waving`: 4 frames
- `jumping`: 5 frames
- `failed`: 8 frames
- `waiting`: 6 frames
- `running`: 6 frames
- `review`: 6 frames

Unused cells must remain transparent.

## QA Checklist

- `$hatch-pet/scripts/validate_atlas.py` passes.
- `$hatch-pet/scripts/make_contact_sheet.py` was used for a contact sheet.
- Every row has the correct frame count.
- Every active row has visible frame-to-frame motion.
- Full opossum head and both ears are visible.
- Yellow-kit `waiting` and `waving` outputs use the complete-ear reference asset, not older cropped-ear cutouts.
- `review` / referee-style outputs must show the full head, rear ear, snout, and tactics-board prop; reject atlas frames or current-state-map cards where the right-facing head is cropped by the top or right edge.
- Share/current-state maps must fit each pet from the full approved frame, with enough top padding that ears do not touch or overlap titles/card edges.
- No schedule board covers the face.
- Jersey mapping matches config.
- China-time schedule text matches `embeddedBoard.displayLines`.
- Transparent pixels have clean RGB.
- Installed path was verified if installation was requested.

## Install Target

Install into:

```text
${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/
  pet.json
  spritesheet-daily-board.png
```

`pet.json.spritesheetPath` must be a relative filename inside the installed pet folder.

Safety requirements:

- `pet-id` must be sanitized to `[A-Za-z0-9_-]+`.
- Do not accept absolute paths from config for install targets.
- Do not overwrite unrelated pet folders unless the user explicitly confirms the same `petId`.
- Do not execute scripts from downloaded configs or fixture files.
