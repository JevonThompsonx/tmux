# tmux Cheatsheet

Quick reference for tmux commands and workflows.

## Start / Attach / Detach

```bash
tmux                      # start new session
tmux new -s name          # new named session
tmux new -d -s name       # new detached session
tmux ls                   # list sessions
tmux attach               # attach to last session
tmux attach -t name       # attach to named session
tmux detach               # detach from current session (prefix + d)
tmux kill-session -t name # kill a session
tmux kill-server          # kill all sessions/server
```

## Sessions

```bash
tmux new-session -s dev -n editor   # session "dev", first window "editor"
tmux rename-session -t old new      # rename session
tmux switch-client -t name         # switch to session
```

## Windows (Tabs)

Prefix = `Ctrl+b` by default.

| Keys | Action |
|------|--------|
| `prefix c` | new window |
| `prefix ,` | rename window |
| `prefix n` / `prefix p` | next / previous window |
| `prefix 0-9` | select window by number |
| `prefix &` | kill window |
| `prefix w` | list windows |

```bash
tmux new-window -n name
tmux rename-window newname
tmux select-window -t :2
tmux kill-window -t :2
```

## Panes (Splits)

| Keys | Action |
|------|--------|
| `prefix %` | split vertically |
| `prefix "` | split horizontally |
| `prefix o` | cycle panes |
| `prefix ;` | last pane |
| `prefix x` | kill pane |
| `prefix z` | zoom pane |
| `prefix {` / `prefix }` | move pane left/right |
| `prefix Space` | toggle layouts |
| `prefix q` | show pane numbers |

```bash
tmux split-window -h      # horizontal split
tmux split-window -v      # vertical split
tmux select-pane -L/-R/-U/-D
```

## Copy Mode

| Keys | Action |
|------|--------|
| `prefix [` | enter copy mode |
| `Space` | start selection |
| `Enter` | copy selection |
| `prefix ]` | paste |
| `q` | exit copy mode |

## Configuration (`~/.tmux.conf`)

```tmux
set -g default-terminal "tmux-256color"
set -g mouse on
set -g history-limit 10000
set -g base-index 1
setw -g pane-base-index 1
bind r source-file ~/.tmux.conf \; display "Reloaded"
```

Reload config: `tmux source-file ~/.tmux.conf` or `prefix r`.

## Sockets / Multiple Servers

```bash
tmux -L name new          # named socket (in /tmp/tmux-UID)
tmux -S /path/to/sock new # full socket path
tmux -L name ls           # list sessions on that socket
```

Note: `-L`/`-S` are **global** options and must come *before* the command
(`tmux -L name new`, not `tmux new -L name`).

## Sessions as Projects

```bash
tmux new -s project -c ~/Projects/project
tmux send-keys -t project "npm run dev" C-m
```

## Troubleshooting

- **"missing or unsuitable terminal: ..."**: install/fallback terminfo for `$TERM`,
  or set `TERM=xterm-256color`.
- **"error connecting to .../default"**: tmux client is trying to reach a server;
  start one with `tmux new` or `tmux start-server`.
- Keep server alive with no sessions: `set -g exit-empty off`

## Useful One-Liners

```bash
tmux kill-session -a              # kill all but current
tmux list-panes -a                # list all panes
tmux capture-pane -p > out.txt    # dump pane to file
```