# mab776 UI add-ons

Loaded by `src/root.tsx` (`MAB776_STYLES` in `<head>`, `MAB776_SCRIPTS` before `</body>`).

| File | What |
|---|---|
| `heroui-palette.css` | HeroUI `--heroui-*` tokens for light / e-ink. GENERATED from the stock build's unused `[data-agent-server-ui] .light{}` block by `patch-ui.py heroui_css()` (portainer repo, `openhands/ui/`). Regenerate after a rebase onto a new upstream version. |
| `light-theme.css` | Light theme behind `<html class="oh-light">` |
| `eink-theme.css` | Colour e-paper layer behind `<html class="oh-light oh-eink">` (16 grey levels, colour on strokes/text only) |
| `theme-toggle.js` | Applies the choice (`localStorage["oh-theme"]` = auto / light / eink / dark, or `?oh-theme=`); adds the "Display (this device only)" row to Settings → Application |
| `eink-guard.js` | Runtime safety net under e-ink: snaps unsafe fills / low-contrast text and logs to `window.__ohEinkGuard` |
| `mode-toggle.js` | ASK/AUTO item in the composer's "+" menu (backend-aware) |

Source of truth while the server runs the stock image: `~/Documents/portainer/openhands/ui/`
(injected into the stock `index.html` at container start). Keep the two copies identical.
