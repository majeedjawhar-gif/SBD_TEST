# SBD ScanTrack - Workspace API Diagnostics

A lightweight Trimble Connect extension for verifying Workspace API integration. It can be installed as a **Project extension**, a **3D Viewer extension**, or both.

## Files

| File | Purpose |
|---|---|
| `index.html` | Diagnostic console (single file, no build step) |
| `manifest.json` | Extension manifest read by Trimble Connect |
| `image.svg` | Extension and menu icon |
| `readme.md` | This document |

## Capabilities

- Automatic connection with `TrimbleConnectWorkspace.connect(window.parent, ...)`
- Per-method test buttons with editable JSON arguments for the `project`, `user`, `extension`, `ui`, `viewer` and `embed` modules
- **Run Read-Only Test Suite**: executes all read-only methods and reports PASS / FAIL with timings
- **Discover API Surface**: lists the methods actually exposed by the host
- **Custom Method Invoker**: call any `module.method` with a JSON argument array
- Live event log (`extension.command`, `extension.accessToken`, `extension.userSettingsChanged`, and any other event) with copy and download as JSON
- Registers a sidebar menu (`SBD ScanTrack` with two sub-menus) when run as a project extension

## Deployment

1. Host the three files in one folder on an **HTTPS** server (for example GitHub Pages, Azure Static Web Apps, Netlify, or an internal web server).
2. Enable **CORS** on `manifest.json` (`Access-Control-Allow-Origin: *`). Connect downloads the manifest before installing.
3. Edit `manifest.json` and replace `YOUR-HOST` in `url` and `icon` with your real address.
4. For local testing: `npx http-server -S -C cert.pem -K key.pem --cors` or use an HTTPS tunnel such as ngrok or Cloudflare Tunnel.

## Installation in Trimble Connect

1. Open a project in Trimble Connect for Browser as a project administrator.
2. Go to **Project Settings > Apps & Capabilities > Add Custom**.
3. Enter the public URL of `manifest.json` and confirm.
4. Enable the extension. Open it from the left navigation (project mode) or the 3D Viewer extension panel.
5. Approve the access-token consent prompt when requested (use the `extension.requestPermission` test).

## Testing notes

- Viewer methods return results only in 3D Viewer mode, and most return data only when a model is loaded.
- `embed.*` methods are intended for the reverse scenario, where your own application hosts Trimble Connect in an iframe. In extension mode they are expected to report as unavailable.
- Methods that are not exposed by the current Connect version are reported as failures; the log shows the exact error.
- Separate manifests (one with `["project"]`, one with `["3dviewer"]`) can be used if you want the two modes installed independently.

## Reference

Trimble Connect Workspace API documentation: https://components.connect.trimble.com/trimble-connect-workspace-api/
