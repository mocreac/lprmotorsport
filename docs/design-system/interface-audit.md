# Interface audit

## Scope and Coverage

Full review of the local game UI, dashboard/sidebar, timing islands, replay, privacy and design-system preview. Stack: plain HTML/JavaScript, shared CSS tokens and locally hosted Phosphor sprite/fonts. Canvas world artwork, production services and native assistive-technology testing are outside this review.

| Domain | Evidence inspected | Result |
| --- | --- | --- |
| Accessibility | Chat accessible names, keyboard focus and send state; FI/EN privacy visibility; icon names | Focusable empty Send retains guarded submission; see detailed copy audit |
| Layout | Dashboard grid, sidebar/touch controls, timing islands; 320–3428px browser states | Empty track and touch overlap fixed |
| Writing | Game instructions, dashboard entry, replay, chat disclosures in FI/EN | Redundant helper prose removed |
| Typography | Local font delivery, timing labels, narrow reference specimens | WOFF2; readable caption sizes; regular 400 retained |
| Colors | Shared text tokens and all 77 rendered swatches | WCAG contrast corrected; restrained brand palette preserved |
| UI | Static and dynamic system-icon sources, disclosure controls | Remaining custom icons replaced with local Phosphor |

## Findings

| # | Severity | Domain | Location | Before | After | Why |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | High | Layout | dashboard.css:364,376,389 | Empty driver column; touch buttons intersected rail | Two-column dashboard; reserved control space and scrollable driver section | Content fits and steering remains reachable |
| 1 | Medium | Colors | design-tokens.css:42 | Tertiary #737373 on #333,2.66:1 | #a0a0a0,4.83:1; secondary #b8b8b8,6.37:1 | Small supporting text remains readable |
| 2 | Medium | Colors | design-system.html:84 | Text color chosen by swatch index | Choose foreground from composited luminance | Alpha ramp and medium shades had unreadable labels; minimum now4.62:1 |
| 3 | Medium | Typography | design-system.css:26; sector-panel.css:11; lap-panel.css:21 | 5–11px labels | 12px reference captions; 14–24px sector panel roles; scales scroll inside rows | Narrow views retain legibility without document overflow |
| 4 | Medium | Writing | index.html:102,205; game.js:386; replay.html:7; i18n.js | Paragraphs describing obvious touch controls, driver links, human/bot labels and replay implementation | Removed redundant prose; shortened consequential notices in both languages | Less UI narration without losing actions, errors or disclosures |
| 5 | Low | UI | design-system.html:22,35; index.html disclosure controls; 3d.css:496 | Hand-drawn specimen icons and native disclosure markers | Shared Phosphor SVGs/carets | Consistent shape, sizing and currentColor |
| 6 | Low | Typography | design-tokens.css:2; server.js | Raw desktop TTF network payloads | Exact same faces served as WOFF2 | Approx.1.58MB reduced to0.58MB across all six faces, outlines preserved |

The top-right timing panel also follows the supplied reference: contiguous sector strip, lap/time row, and full-width delta row. Its 12px outer radius uses an 11px inner edge at the 1px border inset, with square internal divisions. Gray idle/reset, PB green, slower amber, record purple and signed delta states remain connected to timing data.

Layout corrections and exact copy/accessibility source locations are documented in the linked domain evidence below; this report does not claim production-backend or full screen-reader verification.

## Considered but Rejected

| Location | Candidate | Rejected because |
| --- | --- | --- |
| index.html keycaps | Replace literal arrow-key names | Physical keyboard labels are content, not system icons |
| Chat/driver/replay disclosures | Remove every explanatory sentence | Public visibility, recovery limitations and fairness limitations affect decisions |
| design-system.html | Remove specimen descriptions | A reference must explain token roles; gameplay helper copy is different |
| design-tokens.css | Convert all hex values to OKLCH | Existing token notation is consistent; conversion adds churn without solving the measured contrast failures |
| sector-panel.css | Neutralize green/purple sector states | User explicitly requested race-performance semantics |

## Verification

- `npm run build`: passed.
- `npm run test:chat`: passed ownership, shared message persistence, validation, rate limit and expiry checks.
- `git diff --check`: passed.
- `node /tmp/lpr-layout-check/color-verify.cjs`: all 77 swatches minimum 4.6167:1, labels 12px, no page overflow at 1387/390/320px; narrow screenshot inspected.
- FontTools round-trip: all six font outlines, character maps and weights preserved. Chromium `document.fonts.load/check`: six WOFF2 faces loaded, HTTP 200 and `font/woff2`.
- `node /tmp/lpr-layout-check/sector-redesign.cjs`: measured 142px panel height, 12px outer/11px inner radius; idle/reset placeholders, personal/track-record sector states and faster delta verified with local synthetic timing events. Rendered panel screenshot inspected.
- Icon sprite references validated against local symbols; edited JS syntax checked.
- Detailed source/runtime coverage: [icons](audit-icons.md), [copy and accessibility](audit-copy-accessibility.md), [type and color](audit-type-color.md), [layout](audit-layout.md).
- Layout matrix: 320×700,390×844,844×390,1387×979,3428×1600,694×490; no document overflow or touch-control/rail intersection; no page errors. Driver form touch scroll and collapsed rail width verified.
- **Not verified:** native VoiceOver/NVDA, physical iOS/Android and production authentication/network services; true browser200% zoom (694×490 CSS viewport used instead); APCA was not measured. WCAG pair measurements are not a whole-product conformance claim.


## Subsequent feedback implemented

- Driver entry uses Humans/Bots native-radio segments (Humans default), matching Start/Back buttons, dedicated settings navigation and account-derived visibility. Guest/display nicknames do not establish authentication.
- Timing island uses the supplied flush sector/middle/delta layout, design-token colors, matching dividers and concentric12px/11px outer/inner corners at a1px border inset.
- Finish rollover no longer starts a verified session automatically; explicit Restart and guarded R remain available. The verification prepare hook was resetting the car before network success.
- Live delta prefers a complete current-revision PB trace, then falls back to interpolation between PB sector boundaries using stored gate progress or the track's known gate landmarks. Split boundaries stay exact; the between-gate value is estimated. Local preview fixtures can seed this UI only. See [live-delta.md](live-delta.md) for the data rules and tests.
- `node tests/timing.test.cjs` and `npm run test:leaderboard` pass, including sector-boundary interpolation, dense trace precedence, per-lap isolation, finish-line carryover and guarded R restart.

## Verdict

Approve
