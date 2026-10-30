# Interface icon audit

Scope: served game UI (`index.html`), replay (`replay.html`), privacy page (`privacy.html`), and design-system preview (`design-system.html`). Excludes the canvas/world renderer, track/brand artwork, language flags, status dots, and text that names a physical keyboard key or numeric unit.

## Findings and changes

| Location | Before | After | Reason |
| --- | --- | --- | --- |
| `design-system.html:22` | Hand-authored diagonal arrow path in the primary action | Phosphor `arrow-up-right` from the local sprite | Brings the preview into the same icon family as the game; sized 14px alongside 13px text. |
| `design-system.html:35` | Four hand-authored status shapes (check, warning, X, information) | Phosphor `check-circle`, `warning`, `x`, and `info` symbols | Removes the only custom icon drawings in the preview while preserving semantic color and intent; icons render at 18px inside the 30px status tile. |
| `index.html:137`, `index.html:140`, `index.html:268`, `index.html:300`; `3d.css:496` | Browser-native disclosure marker on `<summary>` | Explicit local Phosphor `caret-right`, rotated down for open state | Gives nested disclosure controls a consistent shape and optical size (14px) across browsers. |
| `privacy.html:61`, `privacy.html:95` | Arrow glyph used as prose navigation separator | Natural-language “selecting” / “valitsemalla” wording | The arrow was punctuation between instructions, not an actionable icon. |
| `verified-driving.js:169`, `i18n.js:452` | `F12 → Console` in an error sentence | “Console opened with F12” wording in Finnish and English | Avoids using a directional icon as prose punctuation. |

New symbols are Phosphor regular SVGs from the [official Phosphor core asset catalog](https://github.com/phosphor-icons/core) and use `currentColor`. The project’s existing Phosphor MIT notice remains at `assets/PHOSPHOR-LICENSE.txt`.

## Shared icon path and sizing

- `icons.js:3` allowlists supported Phosphor names; `icons.js:8-18` creates accessible decorative SVG instances from the local sprite URL. Dynamic UI uses this helper in `dashboard.js:45`, `replay.js:11`, `championship.js:96`, and `verified-driving.js:95`. The sound toggle updates the sprite symbol in place in `game.js:33`.
- `components.css:2-10` sets shared SVG dimensions to `1em` and inherits `currentColor`; icon buttons and inline labels therefore scale with their text. The design-system primary-action icon is 14px beside 13px text; semantic marks are 18px in their 30px tile; disclosure chevrons are 14px.
- Replay controls already referenced local Phosphor symbols in `replay.html:3` and `replay.html:7`; no custom replay icon paths needed migration.

## Candidates retained

| Candidate | Decision |
| --- | --- |
| `index.html:205` arrow glyphs inside `<kbd>` | Retained because they label the physical arrow keys the user can press; replacing those key names with decorative icons would reduce fidelity of the keyboard instructions. |
| `replay.html:7` `¼×`, `½×`, `1×`, `2×`, `4×` | Retained because `×` is a playback-speed multiplier, not a close icon. |
| `design-system.html:27` `⌘`, `⇧` keycaps | Retained as keyboard-key examples, not system icons. |
| `index.html` live-presence dot and `design-system.html:22` online dot | Retained as status indicators; neither contains an icon glyph. |
| Four arrow-decorated strings in `i18n.js:50`, `i18n.js:84`, `i18n.js:200-201` | Retained as currently unused legacy translation entries: `rg` found no corresponding UI references. Their visible equivalents use text-only labels or Phosphor SVGs. |
| `i18n.js:351` arrow-key steer label and `i18n.js:399` `Tallennettu ✓` / `Saved ✓` | Retained as inactive translation mappings: exact-string search found no live markup or JavaScript caller. They are not emitted into the served interface. The steering form represents physical key labels; the check-mark mapping currently has no active equivalent. |
| LPR logo, Finnish/UK language flags, game-world and canvas artwork | Retained as brand, locale, or world-rendered assets outside this system-icon pass. |

## Checks

- `npm run build` completed successfully.
- `node --check` passed for the edited JavaScript files (`icons.js`, `i18n.js`, `verified-driving.js`).
- `git diff --check` passed.
- Sprite-reference validation confirmed every local `use href` symbol referenced by the audited HTML exists in `assets/phosphor-icons.svg`.
- Search of audited HTML/CSS found no remaining custom SVG `<path>` icon drawings or system-icon glyphs; literal keyboard labels and speed multipliers above are the only retained glyph candidates.
