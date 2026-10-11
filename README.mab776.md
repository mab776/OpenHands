# mab776 fork of OpenHands (Agent Canvas)

Branch **`mab776-1.20.0`** = upstream tag `v1.20.0` (the version the server runs,
`ghcr.io/openhands/agent-canvas:1.20.0`) + these commits:

| Commit | What |
|---|---|
| `entrypoint: CANVAS_BACKEND_URL` | `docker/entrypoint.sh` routes the browser's `/api`, `/sockets`, `/health`, … to an EXTERNAL agent-server when `CANVAS_BACKEND_URL` is set (the host-native agent on `:7300`); the container is then UI only. Unset = upstream behaviour. |
| `websocket: 5 s reconnect cap` | `src/hooks/use-websocket.ts` caps the reconnect backoff at 5 s instead of 30 s (phone webview resume showed "Unable to connect to server" for up to 30 s). |
| `UI add-ons` | `public/mab776/` (light + colour e-paper themes, e-ink guard, ASK/AUTO "+" menu item, Display row in Settings → Application) loaded from `src/root.tsx`. See `public/mab776/README.md`. |

Companion fork: `mab776/OpenHands-software-agent-sdk`, branch `mab776-1.49.1` (the agent-server).

## How the server uses this today

It still runs the **stock image**; the same changes are applied at container start by
`~/Documents/portainer/openhands/ui/` (`patch-routes.sh`, `patch-ws.sh`, `patch-ui.py`).
This branch is the source-level copy of those patches — keep both in step.

To run this fork instead, build the image (`docker/Dockerfile`, frontend built with
`VITE_BASE_PATH=/canvas`) and drop the three patch calls from the stack's entrypoint.
Verified 2026-10-10: the built frontend + this entrypoint, run in a throwaway container
against the host agent, loaded the e-ink theme, guard, ASK/AUTO item and Display row,
and proxied `/api` to `:7300`, with no console errors.

## Updating to a new upstream version

```bash
git fetch origin --tags
git checkout -b mab776-<ver> v<ver>
git cherry-pick <the commits above>      # drop any upstream has merged
# regenerate public/mab776/heroui-palette.css from the new build (patch-ui.py heroui_css())
git push -u mab776 mab776-<ver>
```
`origin` = upstream (push disabled), `mab776` = this fork. Push over SSH with
`GIT_SSH_COMMAND="ssh -o BatchMode=yes -o IdentityAgent=none"` from non-interactive shells.
