# Layout audit — 2026-10-30

## Scope

Reviewed the running game, startup dashboard, paddock rail, touch controls, sector/lap islands, and dashboard details popover. Layout fixes are in `dashboard.css`; canvas, renderer, and game sizing rules were left intact. The compact sector island is 142px tall in the current preview.

## Confirmed fixes

| Area | Before | After |
| --- | --- | --- |
| Dashboard grid | The startup dialog retained an empty `entry` track. At 1387×979 its columns were 290.6px / 363.2px / 363.2px, pushing Active drivers to x=622. At 3428×1600 the empty track grew to 873.7px. | Removed the unused track and its area. At 1387×979 the active-driver and standings columns are each 514.5px and begin at x=319 and x=846. At 3428×1600 each is 1535px and begins at x=319 and x=1866. |
| Portrait touch controls | At 320×700, the sector island ended at y=313 while steering started at y=290; the rail started at y=398 and overlapped the controls. At 390×844, controls started at y=468 and the rail at y=516, a 24px overlap. Wheel setup controls also consumed mobile space. | At 320×700, steering starts at y=320, ends at y=420, and the rail starts at y=428; the sector island ends at y=313. At 390×844, steering runs y=392–492 and the rail starts at y=516. Steering, brake, and accelerator remain visible; supplementary wheel settings are hidden on touch/narrow layouts. |
| Landscape touch controls | At 844×390, the rail occupied y=210–382 and covered the steering control at y=294–380. Chat composer content also extended below its panel into the driver card. | Rail occupies y=122–282; steering and pedals start at y=294. The composer ends at y=215, with the driver card starting at y=223. The short landscape rail hides nonessential chat metadata and message history to keep composer and driver sections distinct. |
| 200% zoom equivalent | At a 694×490 CSS viewport, the empty grid track remained and consumed 182.8px. | Two 324px columns; no horizontal document overflow. The modal remains vertically scrollable. |
| Collapsed desktop rail | Collapsing the 280px rail left more width unused than necessary. | At 1387×979 the collapsed rail is 44px wide and moves the game shell from x=336 to x=96, reclaiming 240px. The outer corner is 10px; the inset caret button is 7px with a 3px inset. |
| Large desktop | The old grid wasted an increasingly large left track. | At 3428×1600 the game remains expanded to 3060×1380, and the dashboard uses two columns without document overflow. |

## Browser checks

Used isolated Playwright/Chrome against `http://localhost:8000/` at 320×700, 390×844, 844×390, 1387×979, 3428×1600, and 694×490 (half-size CSS viewport as a 200% zoom equivalent). Checked the startup dialog and the game after closing it, then checked the mobile login form, collapsed desktop rail, and dashboard details popover.

- All six sizes had matching document and viewport dimensions; no horizontal or vertical page overflow.
- The dashboard has only `head`, `drivers`, `records`, and `daily` areas. No empty `entry` track remains.
- Steering and pedals did not intersect the paddock rail at any touch viewport. The compact sector island did not intersect the steering row at 320×700 or 390×844; lap and sector islands remained side by side at landscape sizes.
- The mobile chat composer and driver card had an 8px gap at 844×390. Desktop chat messages occupied the remaining rail height (563px at 1387×979). At portrait mobile, messages had 24px at 320×700 and 50px at 390×844.
- With login fields expanded at 390×844, the driver form scroller was 174px high for 488px of content. A simulated touch drag advanced its scroll position, confirming the password and submit controls remain reachable.
- At 1387×979, the details popover ended at x=1363 in a 1387px viewport. The collapsed rail changed the game shell x position to 96px.
- Finnish and English were checked at desktop width; English did not change the viewport overflow measurements. There is no RTL locale in the product, so RTL was not exercised.
- No browser page errors occurred. `npm run build` and `git diff --check` passed.

## Rejected candidates

- Kept the game uncapped at 1320px on very wide displays; the user asked for a larger game view, and the 3428px check showed no document overflow.
- Kept steering, brake, and accelerator controls on mobile. Only the supplementary wheel-settings row is hidden at touch/narrow sizes.
- Kept the paddock rail docked rather than moving it into the page flow. The portrait game and controls remain in their existing viewport layout; reserved bottom space and a landscape offset keep the rail clear of controls.

## Screenshots

Before (initial audit): [320×700](/tmp/layout-320.png), [844×390](/tmp/layout-844.png), [1387×979](/tmp/layout-1387.png).

After (current layout): [320×700 dashboard](/tmp/lpr-layout-final-320x700-dashboard.png), [320×700 game](/tmp/lpr-layout-final-320x700-game.png), [390×844 game](/tmp/lpr-layout-final-390x844-game.png), [844×390 dashboard](/tmp/lpr-layout-final-844x390-dashboard.png), [844×390 game](/tmp/lpr-layout-final-844x390-game.png), [1387×979 dashboard](/tmp/lpr-layout-final-1387x979-dashboard.png), [1387×979 game](/tmp/lpr-layout-final-1387x979-game.png), [3428×1600 dashboard](/tmp/lpr-layout-final-3428x1600-dashboard.png).
