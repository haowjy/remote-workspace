# Repo Viewer

Mobile-friendly read-only web file browser for repositories.

## Features

- Browse repository folders and files
- Preview text files, images, and Markdown (with Mermaid diagrams)
- Upload and manage images via a `.clipboard` panel
- View image galleries in any folder
- Tailscale Serve integration (tailnet-only by default)
- Optional Basic Auth with password

Intentionally **no text editing** to keep remote access simple and lower risk.

## Quick Start

Run in any repo with npx:

```bash
cd /path/to/your/repo
npx @haowjy/repo-viewer
```

### Global Install

```bash
npm install -g @haowjy/repo-viewer
```

Then from any repo:

```bash
repo-viewer
```

By default it serves on `127.0.0.1:18080` with Tailscale Serve (tailnet-only).

## Options

```
repo-viewer [options]

--repo-root <path>    Repository root to expose (default: cwd)
--port <port>         Listen port (default: 18080)
--config <path>       Config file path
--always-hidden <csv> Extra always-hidden path segments (comma-separated)
--show-hidden <csv>   Show hidden/gitignored folders (repo-relative, comma-separated)
--password [pwd]      Enable Basic Auth (optional inline password)
--no-serve            Skip Tailscale Serve (local-only)
--serve               Force Tailscale Serve
--funnel              Enable Tailscale Funnel (public internet, requires --password)
--install             Force dependency install before start
--skip-install        Skip install check
```

### Examples

```bash
repo-viewer                                      # default: Tailscale Serve
repo-viewer --no-serve                           # local-only
repo-viewer --password mypass                    # auth, local-only
repo-viewer --password mypass --serve            # auth + Tailscale
repo-viewer --password mypass --funnel           # auth + public internet
repo-viewer --port 3000
repo-viewer --show-hidden .clipboard,.playwright-mcp
repo-viewer --always-hidden .env,.secrets
```

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `REMOTE_WS_PORT` | `18080` | Listen port |
| `REMOTE_WS_PASSWORD` | | Enables Basic Auth when set |
| `REMOTE_WS_ALWAYS_HIDDEN` | | Extra hidden path segments (comma-separated) |
| `REMOTE_WS_SHOW_HIDDEN` | `.clipboard` | Hidden/gitignored folders to expose (comma-separated) |
| `REMOTE_WS_CONFIG_FILE` | | Config file path override |
| `REMOTE_WS_MAX_PREVIEW_BYTES` | `1048576` | Max file preview size |
| `REMOTE_WS_MAX_UPLOAD_BYTES` | `26214400` | Max upload size |

## Config File

Config file format (default: `.repo-viewer.conf` in repo root):

```bash
REMOTE_WS_PASSWORD=your-password
```

Config file precedence:

1. `--config <path>`
2. `REMOTE_WS_CONFIG_FILE` env var
3. `<repo-root>/.repo-viewer.conf`
4. `~/.config/repo-viewer/config`

## Auth

When a password is set, Basic Auth is required for all routes.
Mutating routes (`POST`/`DELETE`) also require same-origin headers.

When password is set without an explicit serve mode, the launcher defaults to local-only (`--no-serve`). Use `--serve` to override.
`--funnel` requires `--password`.

## Tailscale

Tailscale Serve is enabled by default and stays private to your tailnet.
If Tailscale is missing or disconnected, the launcher exits with guidance.

## Development

```bash
pnpm dev
pnpm dev -- --no-serve
pnpm dev -- --password mypass
```
