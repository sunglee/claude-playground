# claude-playground

A scratch space for experiments with Claude Code mods (plugins of function hooks that hot-reload in a live session).

## Mods

| Mod | Description |
| --- | --- |
| [`token-weather`](mods/token-weather) | A live forecast of the context window, drawn above the prompt. |

Each mod lives in `mods/<name>/` with its own README covering behavior, install and layout.

## Install

This repo is a Claude Code plugin marketplace. In a terminal session:

```
/plugin install <mod> --marketplace sunglee/claude-playground
```

Answer `y` to add the marketplace, then choose a scope (**user** loads it in every session). To update after new commits: `claude plugin update <mod>`, then `/reload-plugins`.

## Adding a mod

Create `mods/<name>/` with `.claude-plugin/plugin.json`, `hooks/hooks.json` and a hooks module, add a `README.md`, then register it in `.claude-plugin/marketplace.json` and the table above.
