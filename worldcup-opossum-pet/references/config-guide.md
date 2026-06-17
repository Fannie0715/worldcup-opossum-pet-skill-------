# Config Guide

## Editable Fields

`petId`: filesystem-safe pet id. Default `oposscoach`.

`petName`: display name shown in `pet.json`.

`showSchedule`: whether to draw the China-time match board into the spritesheet.

`size`:

- `small`: less intrusive, lower readability.
- `normal`: recommended.
- `large`: clearer, more screen presence.

`sharpness`:

- `soft`: closer to original photo texture.
- `crisp`: recommended for Codex pet size.
- `extra-crisp`: stronger outline and contrast, useful on high-DPI screens.

`scheduleBoardStyle`:

- `none`: no board.
- `footboard`: small board near the feet; recommended.
- `tactics-board`: board carried by the pet when the pose supports it.
- `next-match`: show only the next match.
- `full-day`: show all China-date matches, but text may become small.

`animationIntensity`:

- `subtle`: recommended for a calm desk pet.
- `normal`: visible motion while staying unobtrusive.
- `energetic`: stronger motion for REDSkill demos, but still keep the opossum readable.

`stateCopy`: 9 Codex state lines. Keep each line short.

`stateJerseys`: maps each Codex state to a team style key.

`teamStyles`: team-inspired color and notes. Use "team-inspired" styling rather than official logo replication.

## Recommended Defaults

Keep `footboard + full China-date schedule` for the REDSkill demo because it makes the pet feel useful.

If legibility is more important than completeness, use `scheduleBoardStyle: "next-match"`.

Keep animation loops subtle for work states. Codex pets live beside the workspace, so the motion should signal state without becoming distracting.

## State Mapping Guidance

- `idle`, `running`, `review`: calm or focused team styles.
- `waiting`, `waving`: high-recognition bright kit. For the default yellow kit, use `assets/reference/yellow-complete-state.png` as the visual reference so the rear ear remains complete.
- `jumping`: energetic kit with football prop.
- `failed`: kit that supports exhausted/defeated mood.
- `running-right`, `running-left`: same kit to preserve directional continuity.
