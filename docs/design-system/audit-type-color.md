# Typography and color audit

Scope: shared text tokens, live timing islands, and design-system color/type specimens. Plain CSS tokens remain the source of truth; regular 400 and the existing brand/Geist ramps are preserved.

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |
| Medium | design-tokens.css:42 | Tertiary #737373 on surface #333: 2.66:1; secondary #a0a0a0 | Tertiary #a0a0a0: 4.83:1; secondary #b8b8b8: 6.37:1 | Small supporting text needs readable contrast while remaining subordinate. |
| Medium | design-system.html:84 | Swatch text switched from black to white by column index, regardless of actual luminance; alpha ramp especially unreadable | Foreground selected from rendered RGB composited over #171717 | All 77 rendered swatch labels now exceed 4.5:1; minimum measured 4.62:1. |
| Medium | design-system.css:26; sector-panel.css:11; lap-panel.css:21 | Color codes shrank to 5px on phones; timing labels 9–11px | 12px semantic caption token; each color scale scrolls within its row | Readable labels without widening the page; timers retain tabular Stack Sans numerals. |
| Low | design-tokens.css:47 | No root font smoothing | Root antialiased/grayscale smoothing | Consistent regular-weight rendering on macOS. |

Verification: `node /tmp/lpr-layout-check/color-verify.cjs` measured all color swatches at 1387,390,320 CSS pixels: minimum 4.6167:1, all code labels12px, no document horizontal overflow. Full-page screenshots inspected at390px. `npm run build` and `git diff --check` passed. Contrast calculations use WCAG relative luminance, not APCA; no formal whole-product conformance claim. Actual game canvas colors are intentionally outside the UI palette audit.

Rejected: converting the established hex token system to OKLCH would add churn without correcting the contrast root cause. Recoloring green/purple race-performance indicators would contradict requested semantics. Removing educational specimen descriptions would defeat the purpose of a design-system reference; the copy agent owns redundant game helper text instead.
