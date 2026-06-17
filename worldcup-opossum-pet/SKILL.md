---
name: worldcup-opossum-pet
description: Create, configure, install, and iterate a dynamic World Cup themed Codex pet based on a fixed side-profile opossum mascot, using the installed $hatch-pet workflow for Codex-compatible atlas generation, validation, QA, and packaging. Use when the user asks for a REDSkill/Codex Skill that installs an animated pet, customizes opossum pet names, Codex state copy, team-inspired jerseys, state-to-jersey mappings, China-time match schedule boards, pet size/sharpness, dynamic spritesheets, or current-state preview images for the opossum pet.
---

# World Cup Opossum Pet

## Core Positioning

This skill turns a fixed World Cup opossum character into a configurable dynamic Codex pet.

The final pet is not a static illustration. It is an animated Codex spritesheet: 8 columns by 9 rows, with one row per Codex state and multiple frames per row.

This skill is a World Cup configuration layer on top of `$hatch-pet`. For Build and Install modes, use `$hatch-pet` as the authoritative pet pipeline for generation, atlas geometry, validation, contact-sheet QA, motion previews, and packaging. Do not invent a parallel pet format.

The opossum body is locked: side-profile, hands behind back, tired deadpan face, two complete black ears, gray-brown fur, pale snout. Do not redesign it into a cat, panda, cartoon mascot, front-facing toy, or generic animal.

Users may customize the outer layer:

- pet name
- Codex state copy
- team-inspired jersey style
- state-to-jersey mapping
- whether to show a China-time schedule board
- pet size and sharpness
- schedule board style

Do not collapse the pet into a single static image. Static current-state maps are share materials only.

## Output Modes

Choose the smallest useful mode:

1. **Design mode**: output the state table, configuration plan, and preview spec only.
2. **Configure mode**: update or generate `pet.config.json`.
3. **Build mode**: generate a pet package with `pet.json`, spritesheet, config, and validation result.
4. **Install mode**: copy the built pet package into `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>`.
5. **Share mode**: generate a current-state map or REDSkill/Xiaohongshu explanation image.

Installing this skill is not the same as installing the pet. A downloaded skill becomes available to Codex, but the pet appears only after the user explicitly runs Install mode and the final `pet.json` plus spritesheet are written to `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/`.

## References

Read only when needed:

- `references/config-guide.md`: allowed config fields and examples.
- `references/output-contract.md`: required files and validation checklist.
- `references/hatch-pet-integration.md`: how this skill delegates to `$hatch-pet`.
- `config/pet.config.example.json`: default editable user config.
- `assets/reference/yellow-complete-state.png`: canonical complete-ear yellow-kit reference for `waiting` / `waving` share images and rebuild briefs.

## Workflow

### Safety Boundaries

Follow these rules in every mode:

- Do not run remote scripts or download executable code.
- Do not store API keys, tokens, cookies, or user secrets in config files.
- Do not write outside the workspace unless the user explicitly asked to install the pet.
- Install mode may write only to `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/`.
- Sanitize `petId` before using it as a folder name; allow only letters, numbers, hyphens, and underscores.
- Treat `pet.config.json` as data, not code. Never eval config values.
- Use team-inspired colors and props, not official logos, official badges, or trademarked kit replicas.
- If current fixture accuracy is required, verify sources first and summarize the source basis.

### 1. Lock the Opossum

Before changing visuals, confirm the opossum identity is preserved:

- side profile facing right by default
- hands behind back for calm states
- full head and both ears visible
- gray-brown realistic fur
- pale face and long snout
- exhausted football-commentator mood

Never treat the opossum body as a general editable setting. If the user asks to change the animal or face, treat that as creating a different pet, not this one.

For yellow-kit `waiting` and `waving` visuals, use `assets/reference/yellow-complete-state.png` as the canonical complete-ear reference. Do not hand-paint, patch, or approximate the missing rear ear from older yellow cutouts.

### 2. Read or Create Config

If a local config exists, use it. Otherwise start from `config/pet.config.example.json`.

User-facing customization should happen through config, not by editing core scripts first.

Core config groups:

- `petName`
- `showSchedule`
- `size`
- `sharpness`
- `scheduleBoardStyle`
- `stateCopy`
- `stateJerseys`
- `teamStyles`

Default state-to-style mapping:

- `idle`: `argentina`
- `running`: `argentina`
- `review`: `referee`
- `failed`: `peru`
- `waiting`: `brazil`
- `waving`: `brazil`
- `jumping`: `portugal`
- `running-right`: `france`
- `running-left`: `france`

### 3. Map Codex States

The pet must cover all 9 Codex states:

- `idle`
- `waiting`
- `running`
- `review`
- `failed`
- `waving`
- `jumping`
- `running-right`
- `running-left`

Do not drop states just because the visual preview focuses on a few hero poses.

Each state must remain animated:

- `idle`: subtle breathing or blink loop.
- `waiting`: expectant lean or tiny head/body motion.
- `running`: focused working motion, not directional travel.
- `review`: slow suspicious review/tilt loop.
- `failed`: defeated wobble, fall, or hit reaction loop.
- `waving`: paw/gesture wave loop.
- `jumping`: vertical kick/jump loop.
- `running-right`: directional movement to the right.
- `running-left`: directional movement to the left.

### 4. Generate or Rebuild Pet

Before building, load and follow the installed `$hatch-pet` skill:

```text
${CODEX_HOME:-$HOME/.codex}/skills/hatch-pet/SKILL.md
```

Use this skill to prepare the World Cup opossum brief and config; use `$hatch-pet` to produce the Codex-compatible animated pet artifacts.

When building, produce:

- `pet.json`
- `spritesheet-daily-board.png` as the dynamic atlas
- optional `spritesheet-daily-board.webp`
- `daily-board-meta.json`
- `current-state-map.png`
- validation JSON

The atlas must preserve frame counts and row order. A current-state map may show one representative frame per state, but it does not replace the dynamic atlas.

If schedule display is enabled, render the schedule inside the pet spritesheet as a small attached board. Do not rely on native pet click hooks unless the host app explicitly supports them.

The schedule board is embedded at build time. It does not update live unless a separate refresh automation or host integration rebuilds/reinstalls the pet with current fixture data.

For Share mode state maps, compose each state from the full source frame or full approved cutout with generous padding. Do not crop, zoom, or anchor a pet so tightly that the head, rear ear, snout, tail, ball, or prop falls outside the image. Keep the pet below the title baseline; if a state is tall or right-anchored, scale it down instead of letting the head overlap the heading.

### 4.1 Hatch-Pet Compatibility

The build must remain recognizable to `$hatch-pet`:

- final atlas is `1536x1872`, built from `192x208` cells
- row order matches the `$hatch-pet` / Codex pet contract
- all used cells are non-empty and unused cells are transparent
- `pet.json` points to the actual spritesheet path
- validation uses `$hatch-pet/scripts/validate_atlas.py`
- contact sheet uses `$hatch-pet/scripts/make_contact_sheet.py`
- motion preview QA follows `$hatch-pet` expectations
- installation targets `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/`

If local custom scripts post-process jerseys or schedule boards, run them only after the `$hatch-pet` row/state contract is preserved, then re-run `$hatch-pet` validation and visual QA.

### 5. China-Time Schedule Rule

Schedule data must be in `Asia/Shanghai`.

For World Cup games that happen in North America, local afternoon/evening matches often become China-time midnight or morning matches. Select by China date, not source local date.

If the user asks for accuracy or current fixtures, verify against current sources before rebuilding fixture data.

### 6. Visual QA

Check before delivery:

- full head and both ears visible in every state
- yellow-kit `waiting` / `waving` states match the complete-ear reference asset
- no body parts cropped
- Share mode maps keep the complete head and both ears clear of title text and card edges
- schedule board does not cover the face
- board text is short enough to read at pet size
- `running-left` and `running-right` point in correct directions
- animation rows have visible frame-to-frame variation
- transparent background validates cleanly
- installed pet directory exists if install was requested
- installed files are compatible with `$hatch-pet` package expectations

## Response Style

When reporting results, keep it practical:

- what changed
- what states are mapped to which team styles
- where the files are
- whether validation passed
- whether the user needs to restart or switch pets to refresh cache

Avoid overclaiming that a downloaded skill automatically controls Codex. It can install a pet after the user runs the skill, but native pet selection/cache may still require a refresh or restart.
