# replay-theater

Step through the file edits Claude made in the last turn, one diff at a time.

After a turn that changed files, a band above the prompt shows `▶ Replay: 3 edits (press r)`. Press `r`, click **Replay**, or run `/replay` to open a pane showing one edit at a time:

- the file path, the tool that made the change (`Edit`, `MultiEdit`, `Write`), and `+added` / `-removed` counts
- a step strip (` 1  2  3 `) with the current step highlighted
- a colored line diff (green for added, red for removed, dim for context), capped at 12 lines per step with a `… N more lines` footer

| Button | Hotkey | Action |
| --- | --- | --- |
| ◀ Prev | `p` | Previous edit |
| Next ▶ | `n` | Next edit |
| Close | `c` | Close the replay (`Esc` also works in the pane) |

If the terminal is too narrow for a pane, the replay is drawn in the band above the prompt instead.

## How it works

- `tool.call` records every `Edit`, `Write` and `MultiEdit` before it runs. It never blocks or alters the call, and recording errors are swallowed.
- A `MultiEdit` becomes one step per sub-edit (`edit 2 of 4`). A `Write` reads the existing file first, so it shows a real diff (`rewrite`) or the full content (`new file`).
- Diffs are an LCS line diff with one line of context around each change. Inputs over 400 lines fall back to all-removed then all-added.
- `turn.start` clears the pending list. On a main-loop `turn.complete`, the pending edits become the replay. Subagent turns are ignored, and a turn with no edits keeps the previous replay.
- `/replay` is registered on `session.start`. The band renders through `ui.render` on `AbovePrompt`, and the pane through `ui.render` on `Pane`.

## Install

```
/plugin install replay-theater --marketplace sunglee/claude-playground
```

Answer `y` to add the marketplace, then choose a scope (**user** loads it in every session). The mod is active as soon as the install finishes.

To update after new commits: `claude plugin update replay-theater`, then `/reload-plugins`.

## Layout

```
.claude-plugin/plugin.json   # plugin manifest
hooks/hooks.json             # lists the hook modules to load
hooks/replay-theater.mjs     # the mod
```

## Known limitations

- Only the last turn's edits are kept, and only in memory, so a restart clears them.
- Edits made by Bash commands (`sed`, `git apply`, ...) and by subagents are not recorded.
- Replay state is module state, so a hot reload clears it (unlike [token-weather](../token-weather/README.md), which keeps history in host-held plugin state).
