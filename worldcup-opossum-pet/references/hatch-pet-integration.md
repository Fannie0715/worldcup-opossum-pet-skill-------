# Hatch-Pet Integration

This skill is not a replacement for `$hatch-pet`. It is a themed configuration layer that prepares a World Cup opossum brief and editable config, then delegates the animated pet build to `$hatch-pet`.

## Required Dependency

Expected local dependency:

```text
${CODEX_HOME:-$HOME/.codex}/skills/hatch-pet/SKILL.md
```

If `$hatch-pet` is unavailable, stop before Build or Install mode and tell the user to install it first. Design and Configure modes can still run without it.

## What Skill Installation Does

Installing `worldcup-opossum-pet` only makes the Codex skill available. It does not automatically display a pet, change Codex settings, or attach a native click hook.

To display the pet, the user must explicitly run an install request such as:

```text
Use $worldcup-opossum-pet 安装世界杯负鼠宠物。
```

Then this skill should:

1. Check that `$hatch-pet` is installed.
2. Build or rebuild the Codex-compatible pet package through `$hatch-pet`.
3. Write only the final pet package to `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/`.
4. Tell the user to refresh/restart/switch pets if the Codex app caches pet assets.

If the user only installs the skill folder and never runs an install request, nothing will appear in the pet UI.

## Schedule Display Limit

The China-time schedule board is embedded into the generated spritesheet. It is not a live web widget unless the host Codex pet system later exposes a native pet overlay or click hook.

For accurate daily schedules, rebuild or refresh the pet package with current fixture data before installing. Do not claim the board updates itself unless a separate automation or host integration is actually installed.

## Delegation Rule

Use this skill for:

- locked opossum identity
- REDSkill positioning
- editable `pet.config.json`
- World Cup state copy
- team-inspired jersey mapping
- China-time schedule board behavior

Use `$hatch-pet` for:

- base pet generation
- 9 Codex state rows
- dynamic row-strip generation
- `running-left` derivation decisions
- atlas composition
- transparent-pixel cleanup
- validation
- contact sheet
- motion previews
- final pet packaging

## Build Handoff

When a user asks to build or install the pet, create a compact handoff for `$hatch-pet`:

```text
Use $hatch-pet to create/repair a Codex-compatible animated pet.

Pet name: <petName>
Description: A side-profile, hands-behind-back, tired World Cup opossum Codex pet with configurable team-inspired jerseys and a small China-time match board.
Reference images: <locked opossum source images or existing spritesheet/contact sheet>
Style: realistic side-profile opossum, pet-safe, transparent atlas, no official logos.
State mapping: <stateJerseys from config>
State copy: <stateCopy from config>
Schedule board: <showSchedule + scheduleBoardStyle + timezone>
Output requirements: 8x9 atlas, 192x208 cells, validation, contact sheet, motion previews, installable pet.json.
```

## Post-Processing Rule

If schedule boards, sharpness, or jersey recolors are applied after `$hatch-pet` output, always re-run:

```text
${CODEX_HOME:-$HOME/.codex}/skills/hatch-pet/scripts/validate_atlas.py
${CODEX_HOME:-$HOME/.codex}/skills/hatch-pet/scripts/make_contact_sheet.py
```

Do not call the pet complete until validation and visual QA pass after post-processing.
