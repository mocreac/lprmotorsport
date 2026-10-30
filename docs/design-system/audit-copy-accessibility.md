# Copy and accessibility audit

**Scope:** game HUD and driving controls, paddock chat, driver entry and standings dialogs, daily race dashboard, replay/live spectating and report flow, privacy page, and the design-system reference.

**Skills applied:** `better-writing`, `better-accessibility`, and `emil-design-eng`.

## Decisions

| Area | Change or decision | Reason |
| --- | --- | --- |
| Driving | Removed the phone paragraph that explained the visible steering arrows and pedals. Kept the keyboard keycaps because the mappings are not otherwise visible. | Visible controls communicate touch behavior; key mappings remain useful discovery. |
| Reset prompt | Removed “Press the accelerator to start driving” on reset. The accelerator control is visible; crash, lap, connection, and other event notices remain. | Avoids an instructional overlay that repeats the interface. |
| Driver list | Removed the sentence telling people to press driver names and explaining that chat remains available. The names remain links and chat is always present in the rail. | The copy described visible interactions and duplicated persistent layout. The no-drivers and loading/error states remain. |
| Series choice | Removed the prose descriptions that restated “human” and “bot.” Kept the separate-leaderboards and admin-change consequence. | The labels are self-describing; the series consequence affects the choice. |
| Guest entry | Shortened the paragraph while retaining public result visibility, no guest championship points, live spectating, and the account option to keep laps. | These are material result and privacy consequences. |
| Signed-in entry | Kept the live-spectating disclosure, leaderboard/data explanation, and password recovery warning. | They describe visibility, processing, and an irreversible account consequence. |
| Replay | Replaced the implementation detail paragraph (sampling rate, interpolation, input resolution) with the shorter fairness notice that a recording does not establish whether a human or bot drove. | Playback controls and view options are labeled; the retained notice prevents overclaiming from a replay. Public visibility and playback-delay copy remains in live spectating. |
| Chat | Kept the public-message notice, accessible input label, localized icon-button name, and actionable signed-out/network/send statuses. Routine connected status stays empty in the live region. | The public notice is a privacy disclosure; labels and errors support access and recovery. |
| Privacy | Kept both complete language versions and all existing collection, retention, provider, replay, account, and rights disclosures unchanged. | These are necessary privacy information, not helper copy. |
| Design system | Kept the explicit English-only reference and its explanatory token notes. | It is a standalone design artifact, documented as English-only; the game remains FI/EN. |

## Accessibility behavior retained or improved

- Chat’s visible “Message” label is visually hidden but remains associated with the input.
- The icon-only Send button retains localized `aria-label` and `title` values and its Phosphor arrow-up icon.
- Empty and whitespace-only input leaves Send focusable with `aria-disabled="true"`; a trimmed message makes it ready, and an active request uses native `disabled` until completion. This preserves keyboard discoverability while preventing an empty submission.
- The chat status region remains stable and announces actionable states; ordinary connected state is blank.
- Replay controls remain native buttons/selects with labels, while canvas descriptions and icon-button names remain available to assistive technology.
- No motion was added to keyboard actions.

## Candidates reviewed and retained

- The public chat warning and replay/leaderboard public-visibility statements remain because they disclose who can see user content or results.
- Guest and account disclosures about public laps, series points, live viewing, and account retention remain.
- The daily competition schedule and points breakdown remain because they define competition rules.
- Loading, empty, connection, save, authentication, and crash states remain where they provide status or recovery.
- Keyboard legends remain because the key mapping is not inferable from the touch controls.

## Verification

Checks are run against the localhost development preview using only its local demo API. Browser requests to other hosts are blocked during the local-demo checks.

- FI/EN game and replay strings, the simplified driver list, removed reset instruction, chat input accessible name, and Send icon/name were inspected in browser.
- Keyboard focus reaches chat input and the empty Send control; the input label and button name are available to the accessibility tree.
- Empty, whitespace, trimmed text, busy, failed send, and successful clear states were checked; no empty message was posted.
- Privacy page retains exactly one complete language version at a time and its native FI/EN buttons update the document language.
- The design-system reference declares itself English-only.
- Build, JavaScript syntax checks, and `git diff --check` were run.

## Source locations

Line numbers below refer to the audited working tree at the time of this pass.

- `index.html:102` contains the shortened guest disclosure; `index.html:120` retains the account live-view disclosure. The chat's visually hidden input label and icon-only Send button are at `index.html:85-89`.
- `game.js:386-387` clears and hides the reset notice; event notices still use `game.js:397-401`.
- `chat.js:16-20` synchronizes readiness, the neutral state, and `aria-disabled`. The chat submit handler trims and rejects empty input before authentication or posting (`chat.js:113-141`).
- `replay.html:7` contains the concise fairness disclosure and labeled replay controls.
- The new FI/EN mappings are in `i18n.js:20`, `i18n.js:84`, and `i18n.js:122`.

## Verification details

Commands and outcomes:

- `NODE_PATH=/tmp/lpr-layout-check/node_modules node /tmp/lpr-layout-check/chat-send-state-check.cjs` — passed. Checked empty/whitespace neutral states, Enter on whitespace/empty does not post, Tab from the labeled input reaches the localized empty Send button, ready/busy/error/success-clear styling, and two mock local chat posts only.
- `NODE_PATH=/tmp/lpr-layout-check/node_modules node /tmp/lpr-layout-check/copy-a11y-check.cjs` — passed. Checked FI/EN guest and replay text, removed helper-copy nodes, empty initial notice, associated input label, replay fairness text, full-language privacy visibility, and English-only design reference.
- `NODE_PATH=/tmp/lpr-layout-check/node_modules node /tmp/lpr-layout-check/language-selector-check.cjs` — passed. Checked ArrowDown navigation, Escape closes and restores focus, outside click closes, FI/EN persistence, all three icon-only triggers, and both local flag assets.
- All browser checks used `http://localhost:8002/`; external network requests were blocked and none were made. Chat POSTs were intercepted locally; no real account or message service was used.
- `node --check game.js && node --check chat.js && node --check i18n.js` — passed.
- `npm run build` — passed; generated `dist/`.
- `git diff --check` — passed.

These were browser DOM and keyboard checks. No screen reader was run, so this pass does not claim screen-reader verification.

## Guest series control follow-up

- `index.html:104-112` now uses a native radio group with “Ihmiset / Botit” and Human selected by default. Existing FI/EN dictionary entries localize the legend and options.
- `leaderboard.js:283-284` reads the checked radio while preserving the `human` and `bot` values sent to the existing guest-session endpoint.
- `dashboard.css:397-409` styles it as a two-segment control with visible keyboard focus. Guest Start is centered and sentence case; Back is a neutral button. Both measure 40 px high and the nested control radii are concentric.
- `NODE_PATH=/tmp/lpr-layout-check/node_modules node /tmp/lpr-layout-check/guest-series-check.cjs` — passed in FI/Human and EN/Bot. Tab reached the default radio, ArrowRight selected Bot, local guest requests and resulting identities retained the selected series, and no external requests were made.
- `node --check leaderboard.js && node --check i18n.js`, `npm run build`, and `git diff --check` — passed.
