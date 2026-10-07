# token-weather

Shows a one-line band above the prompt that reports how full the context window is, as a weather forecast:

| Context used | Forecast |
| --- | --- |
| < 25% | ☀️ Clear |
| < 50% | 🌤️ Cloudy |
| < 75% | 🌧️ Showers |
| < 90% | ⛈️ Storm |
| ≥ 90% | ⚡️ Compact soon |

The band also shows token counts (`used / window`) and, when the terminal is at least 60 columns wide, a sparkline of the last 12 readings plus the change since the previous turn (`▲ +98.3k last turn`).

## How it works

- Takes a reading on `session.start` and after each main-loop `turn.complete` (subagent turns are ignored).
- Keeps the last 12 readings in host-held plugin state, so history survives a hot reload.
- Renders through the `ui.render` hook on the `AbovePrompt` component, and steps aside while a survey is showing.

## Install

```
/plugin install token-weather --marketplace sunglee/claude-playground
```

Answer `y` to add the marketplace, then choose a scope (**user** loads it in every session). The mod is active as soon as the install finishes.

To update after new commits: `claude plugin update token-weather`, then `/reload-plugins`.

## Layout

```
.claude-plugin/plugin.json   # plugin manifest
hooks/hooks.json             # lists the hook modules to load
hooks/token-weather.mjs      # the mod
tests/token-weather.test.ts  # test using claude-code/testing
types/index.d.ts             # types for the mod API
```
