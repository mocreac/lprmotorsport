# Feedback audit — 2026-10-30

This checklist reconciles the visual requests accumulated during the local UI review. “Verified” means the current source and/or the checks listed below support the result. It does not imply a complete accessibility, browser, or design-token audit.

## Verified

| Request | Result and evidence |
| --- | --- |
| Typography hierarchy and data figures | Stack Sans Headline is the display/data face; Inter is the body face. The preview display weight and tracking were softened. Lap, sector, replay, championship, and standings times use monospaced/tabular data styling. Speed remains Stack Sans at the latest 64px light setting and has tabular numerals. |
| Local fonts/privacy | Fonts are served from `assets/fonts/`; no Google Fonts request is required. |
| Phosphor interface icons | Interface symbols use the local regular Phosphor sprite/helper; license notice is in `assets/PHOSPHOR-LICENSE.txt`. The arrow/letter characters in physical keyboard keycaps are input labels, not interface icon substitutions. |
| Dark palette and brand | UI surfaces use the Geist-derived neutral tokens; the LPR brand token is `#03BC7E`. Semantic warning/error colors remain separate. The game-world renderer colors were not included in the neutral surface migration. |
| Nested corner rule | The design-system guidance documents `outer = inner + inset padding`. The preview nested surfaces and sector panel use inset-adjusted radii. The shared series control now uses outer 8px / inner 6px with a 2px inset, rather than matching the enclosing card’s 10px radius. |
| Game panel and dashboard layout | Full-width game area, flush upper edge, rounded lower game corners, dashboard header alignment, common outer card surfaces, divider-only championship columns, and Top 10 spacing are present. Existing layout audit documents the 320×700, 390×844, 844×390, 1387×979, 3428×1600, and 694×490 viewport measurements. |
| Sidebar actions and language | Current collapsed gameplay rail computes trophy and restart as `display:grid`. When the dashboard/settings dialog is open, game-only actions are hidden. The single language selector is retained by Settings and can be portaled into the open modal; previous audit verified FI/EN switching and return to the rail. |
| Figma-inspired drive controls | Four drive settings (traction, display, sound, dimming) remain together in a collapsible panel. The latest centered telemetry arrangement has left brake/right throttle bars and a fixed 3ch speed slot. Speed text width measured 125.75px for `0`, `9`, and `115`; computed font is 64px with tabular numerals. The panel-open short-landscape check hid both sector and telemetry strips; closing it restores them. |
| Last completed lap | A `lpr-sector-lap-completed` event reveals a 36px `VIIMEISIN KIERROS` / `LAST LAP` row in the sector panel. It retains its value through the following lap and invalid-lap reset, then clears on a fresh lap reset/session. FI/EN labels update at runtime. Both Last Lap and Delta rows measured 36px with 20px Stack Sans Headline 400 tabular values at desktop, landscape, and portrait-phone widths. |
| Pedal rendering | The physics input target/ARIA value updates immediately; only the visual fill is eased in the render helper. In a live browser check, throttle fill was 46.5% after about 50ms and 95.6% after about 210ms while ARIA already reported 100. With `prefers-reduced-motion: reduce`, the fill snapped to 100%. Brake and throttle use solid fills, in their requested directions. |
| Press feedback | The final dashboard button active rules scale to `.95`; held keyboard keycaps scale to `1.05`. Keycap pressed state clears on keyup, blur, and document hiding. Reduced-motion disables the button transform. |
| Phone/landscape viewport | At 320×700 and 390×844, the current game surface fills the remaining width after the collapsed rail and is 680px tall; lap/sector panels end at y=365 and speed telemetry occupies y=590–662. Document width equals viewport width at both sizes. At 844×390, the rail actions remain visible; opening the drive settings panel hides sector and telemetry, preventing overlap. |
| Local build | `npm run build` and `git diff --check` passed after the latest source changes. |

## Unresolved or only partially checked

- Exact, pixel-for-pixel parity with the latest Figma revisions was not re-established in this audit. The implementation follows the latest referenced centered red/green meter composition, fixed-width speed slot, and collapsible settings-panel requests; do not treat this as a fresh measurement of every Figma node’s geometry, color, or radius.
- The concentric-corner formula is documented and applied in the checked components, but there is no repository-wide radius linter. Unvisited legacy or future nested components can still violate it.
- The small viewport checks above cover the game shell, panel positions, and document width. They do not constitute a complete interaction pass for every touch control, modal, chat state, or every mobile/browser engine.
- Reduced-motion behavior was checked on the pedal fill and button feedback. No full motion/accessibility audit was run.
- Parent/other-agent work owns auth/chat behavior, timing/delta logic, translation coverage, and most icon migrations. Refer to `audit-icons.md`, `audit-type-color.md`, and the relevant feature tests for those claims; this layout/HUD audit does not independently revalidate their business logic.

## Focused checks performed in this follow-up

- Playwright against `http://localhost:8000/`: 320×700 and 390×844 game geometry; 844×390 collapsed rail actions, fixed speed slot, eased throttle fill, settings-open collision handling.
- Playwright with normal and reduced motion preferences: stable speed slot for `0`, `9`, `115`; gradual versus immediate pedal fill behavior.
- Playwright synthetic completion/start/invalid-reset/fresh-reset events at 1387×979, 844×390, and 390×844; checked Last Lap persistence, clearing, FI/EN label update, equal row height, and panel bounds. Screenshot: `/tmp/lpr-lastlap-visible.png`.
- `npm run build`
- `git diff --check`

## Auth, chat, and driving follow-up

- Account-only composer: signed-out and guest sessions hide the complete composer; authenticated account sessions show it. The current compact-sidebar browser regression covers sign-in/out, guest, reload, safe text rendering, localization, and mobile layout. `NODE_PATH=/tmp/lpr-layout-check/node_modules npm run test:chat-browser` passed.
- Guest form: caret Back shares the heading row; privacy text follows the heading before nickname; Start playing is centered. Guest, Top 10, and Daily series controls share the same inset two-column component. Focused FI/EN and keyboard-radio checks passed.
- Traction control: preference is saved independently of vehicle state and reapplied after local reset and verified world creation. The game-loop reset regression and vehicle torque-limiting test pass. Keyboard steering assistance can make its effect subtle; no physics tuning was changed.
- Delta: see `live-delta.md` for the latest implementation and validation. Sector totals alone are not a continuous same-position reference.
- Time typography follow-up: removed generic monospace overrides; `numeric-typography.css` applies Stack Sans Headline 400 with lining/tabular figures to lap, sector, delta, leaderboard, Daily, and replay values. Browser measurement of the actual local WOFF2 gave equal 15.125px advances for every digit 0–9 at the checked size; proportional figures differed. Build and computed-style checks passed.
