# tmux

> prefix: move-pane -t 0:2 (0: session, 2: window number)

> Default prefix is `Ctrl-b`. Below, `prefix` means press the prefix first, then the key.

## Session

| Command / Key | Description |
| --- | --- |
| `tmux new -s name` | Create a new named session |
| `tmux ls` | List sessions |
| `tmux attach -t name` | Attach to a session |
| `tmux kill-session -t name` | Kill a session |
| `prefix d` | Detach from current session |
| `prefix s` | List/switch sessions |
| `prefix $` | Rename current session |
| `prefix (` / `prefix )` | Previous / next session |

## Window

| Command / Key | Description |
| --- | --- |
| `prefix c` | Create a new window |
| `prefix ,` | Rename current window |
| `prefix &` | Kill current window |
| `prefix n` / `prefix p` | Next / previous window |
| `prefix 0..9` | Switch to window by number |
| `prefix w` | List windows |
| `prefix f` | Find window by name |

## Pane

| Command / Key | Description |
| --- | --- |
| `prefix %` | Split pane vertically (left/right) |
| `prefix "` | Split pane horizontally (top/bottom) |
| `prefix ←↑↓→` | Move to pane in direction |
| `prefix o` | Cycle through panes |
| `prefix q` | Show pane numbers |
| `prefix x` | Kill current pane |
| `prefix z` | Toggle pane zoom (fullscreen) |
| `prefix {` / `prefix }` | Swap pane with previous / next |
| `prefix space` | Cycle pane layouts |

## Copy mode & scroll

| Command / Key | Description |
| --- | --- |
| `prefix [` | Enter copy mode (then scroll/navigate) |
| `prefix ]` | Paste copied text |
| `q` | Quit copy mode |
| `↑ ↓ ← →` | Move cursor |
| `PgUp` / `PgDn` | Scroll up / down one page |
| `Ctrl-u` / `Ctrl-d` | Scroll up / down half a page |
| `g` / `G` | Jump to top / bottom of buffer |
| `/` / `?` | Search forward / backward |
| `n` / `N` | Next / previous search match |
| `space` | Start selection |
| `Enter` | Copy selection and exit copy mode |

> Keys in copy mode follow the `mode-keys` setting (`emacs` by default; the keys above assume `setw -g mode-keys vi`).
