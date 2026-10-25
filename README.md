# Surehand MCP for Adobe® After Effects®

Surehand MCP lets Claude work inside the Adobe After Effects you have open, and shows you what it did. Every edit
comes back with an exact list of what changed in the project and is one undo step. Structural changes (un-precompose,
deep duplicate) come with pixel proof that the picture didn't change. Preflight checks a comp and its render against
your own delivery spec, the stress test pushes worst-case text and numbers through a template, and farm renders are
checked frame by frame.

This plugin is the Claude Code package of [Surehand MCP](https://github.com/ConformLabs/surehand-mcp) (MIT, by
Conform Labs). It contains the Surehand MCP server and four skills that teach Claude to use it well.

## Requirements

- Adobe After Effects 2025 or 2026 (older versions: best effort), macOS or Windows.
- Node.js 18 or newer on your PATH (`node`).

## What it contains

| Part | What it is |
|---|---|
| `server/surehand-ae.mjs` | The Surehand MCP server: the release file from [ConformLabs/surehand-mcp releases](https://github.com/ConformLabs/surehand-mcp/releases) for this plugin's version, built from the source in that repository and checked against the release's SHA-256. Claude Code starts it with `node`. A workflow here picks up each new stable release. |
| `skills/` | Four Agent Skills: working in After Effects, delivery checks, safe restructuring, exact timing and layout. |

## What it runs, changes and connects to

The server runs on your computer and talks to After Effects through a small extension:

- **First start:** it creates `~/.surehand/ae/config.json` with a random access token, and copies its signed After
  Effects extension into your user Adobe CEP extensions folder. Restart After Effects once to load it
  (**Window › Extensions › Surehand MCP**). If an older Surehand installation exists in `~/.aemcp`, it is moved to
  `~/.surehand/ae`, and AI app settings that pointed at the old server are pointed at the new one.
- **Connection:** the extension listens on `127.0.0.1` only and requires the token. Nothing listens on the network.
- **What Claude can do:** it runs After Effects scripts (ExtendScript) and Surehand's tools in your open project. In
  the Surehand panel in After Effects you choose Full, Ask (approve each change) or Read-only, and can pause the AI.
  Every change is logged locally in `~/.surehand/ae`.
- **Network requests Surehand makes:** an update check against GitHub releases at most once a day (turn it off with
  `"updateCheck": false` in `config.json`); ffmpeg for Preflight's file checks, downloaded only after you agree
  (SHA-256 pinned); Cloudflare's `cloudflared` only if you set up ChatGPT on the web.
- **No data is collected** by Conform Labs. What Claude sends to Anthropic (your prompts and what it reads from the
  project) is covered by your Claude terms. Privacy: [conformlabs.co/privacy](https://conformlabs.co/privacy/).

## Learn more

- Website and installers for other AI apps: [conformlabs.co/get](https://conformlabs.co/get)
- Documentation: [the wiki](https://github.com/ConformLabs/surehand-mcp/wiki)
- Issues: [github.com/ConformLabs/surehand-mcp/issues](https://github.com/ConformLabs/surehand-mcp/issues)

Adobe and After Effects are registered trademarks of Adobe. Surehand MCP is not affiliated with or endorsed by Adobe.
