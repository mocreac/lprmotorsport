# LPR Motorsport UI foundations

The first UI contribution establishes typography and color tokens, and includes a standalone visual reference at [`/design-system.html`](../../design-system.html). The design-system reference is intentionally English-only; the game interface itself supports Finnish and English.

## Design decisions

- **Headings:** Stack Sans Headline, used for page and section headings and driving data.
- **Body and controls:** Inter.
- **Font delivery:** Both families are self hosted under `assets/fonts/`; page visits do not request fonts from Google or another font CDN. Font files are provided under the SIL Open Font License by Google Fonts. Retain the upstream OFL notices when redistributing or modifying the font files.
- **Neutral ramp:** Geist inspired dark gray values for surfaces, borders, and text. Gray alpha tokens are reserved for translucent overlays.
- **Brand:** Green ramp centered on LPR green `#03BC7E`.
- **Semantic ramps:** Purple, amber, red, and blue are available for category, warning, error, and information states.
- **Icons:** Use Phosphor icons only for interface iconography. Use the regular weight SVGs inline or from the project’s shared icon component; never substitute Unicode glyphs, emoji, or a second icon set.
- **Icon license:** The preview uses the regular Phosphor SVG paths, covered by the MIT license in [`assets/PHOSPHOR-LICENSE.txt`](../../assets/PHOSPHOR-LICENSE.txt).
- **Concentric corners:** For a nested surface, set `--radius-inner: calc(var(--radius-outer) - var(--inset-padding))`. Equivalently, `outer radius = inner radius + inset padding`. Use this on every nested component; equal radii on nested surfaces are not allowed. The preview demonstrates the rule on swatches and status cards.
- **Keycaps:** Use semantic `<kbd class="keycap">…</kbd>` elements for physical-key hints. The keycap treatment is reusable and should only be shown when keyboard input is relevant.
- **Numerals:** Use Stack Sans Headline for prominent UI numbers. Use tabular monospaced numerals for lap times and other fixed-width telemetry.
- **Type scale:** Use the shared 4 px based scale in `design-tokens.css`: 12, 14, 16, 18, 20, 24, 28, 32, 40, and 48 px, plus a responsive 56–108 px display range. Prefer the semantic aliases `--text-caption`, `--text-label`, `--text-body`, `--text-title-small`, `--text-title-medium`, `--text-title-large`, and `--text-display`. Body copy uses a 1.5 line height; headings use 1.2; compact display uses 1.1.

## Token names

Primitive colors use `--{color}-{step}` with steps `50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950`. Semantic aliases such as `--background-1`, `--surface-1`, `--text-primary`, `--border`, `--brand`, and `--status-danger` should be preferred in components.

## Review

Serve the repository (`npm run dev`) and open `/design-system.html`, or open the static HTML file directly. No app dependencies are required for the preview. The real game is served at `/`; to see its dashboard with working demo sign-in and populated local standings, open `/?demo=1` and use the local demo credentials shown in the sign-in panel (`Mika Demo` / `demo1234` by default). Demo credentials and sample data use separate browser storage keys from real local accounts.

The demo API is available only in development, from a loopback request whose referring page has `?demo=1`. It reads only `DEMO_DRIVER_NAME`, `DEMO_ACCOUNT_PASSWORD`, and `DEMO_LAP_TIME` from the ignored `.env` so the local page can display the demo login details; standings and chat content are synthetic fixtures. It returns 404 in the production server.

The localhost preview may read only `DEMO_DRIVER_NAME`, `DEMO_ACCOUNT_PASSWORD`, and `DEMO_LAP_TIME` from the ignored `.env` file. Copy `.env.example` to `.env` to set demo values. The `/api/dev/design-preview` endpoint returns sanitized sample rows and the intentionally visible local demo login values, and is disabled by the production server. It never returns any other environment variables or credentials.

## Follow-on UI branches

Keep later contributions narrow and independently reviewable: migrate remaining screen-level iconography to Phosphor, then migrate component colors and spacing to semantic tokens. Apply the typography, numeric, and concentric-corner rules in every touched component. Avoid combining broad migrations with token naming changes.
