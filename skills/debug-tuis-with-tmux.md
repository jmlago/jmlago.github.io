# Skill: Debug TUIs with tmux

Use tmux to visualize and interact with Terminal User Interfaces (TUIs) like nvim, lazygit, debuggers, and REPLs. This creates a feedback loop that allows Claude to "see" and "control" interactive applications.

## When to use

- Debugging code step-by-step with pdb, gdb, or other debuggers
- Interacting with REPLs (Python, Clojure, Node, ghci)
- Debugging TUI configurations (nvim plugins, lazygit, etc.)
- Testing keybindings and verifying they work correctly
- Guiding users through unfamiliar interactive applications

## Core commands

```bash
# Capture what's on screen
tmux capture-pane -t shared -p

# Send keystrokes
tmux send-keys -t shared "text"
tmux send-keys -t shared Enter
tmux send-keys -t shared C-c  # Ctrl+C
```

## Feedback loop pattern

```bash
# The core cycle: send → wait → capture
tmux send-keys -t shared "$COMMAND"
sleep 0.3
tmux capture-pane -t shared -p
```

This is the fundamental pattern: send input, wait for UI to update, capture the result.

## Debuggers

The most powerful use case. Full debugging sessions with variable inspection, breakpoints, and stack traces.

### Python (pdb)

```bash
# Start debugging
tmux send-keys -t shared "python -m pdb script.py" Enter

# Navigation
tmux send-keys -t shared "n" Enter      # next line
tmux send-keys -t shared "s" Enter      # step into
tmux send-keys -t shared "c" Enter      # continue
tmux send-keys -t shared "r" Enter      # return from function
tmux send-keys -t shared "u" Enter      # up stack frame
tmux send-keys -t shared "d" Enter      # down stack frame

# Inspection
tmux send-keys -t shared "p variable" Enter       # print variable
tmux send-keys -t shared "pp locals()" Enter      # pretty print locals
tmux send-keys -t shared "pp vars(obj)" Enter     # object attributes
tmux send-keys -t shared "bt" Enter               # backtrace
tmux send-keys -t shared "l" Enter                # list source code
tmux send-keys -t shared "ll" Enter               # long list (full function)

# Breakpoints
tmux send-keys -t shared "b file.py:42" Enter     # set breakpoint
tmux send-keys -t shared "b function_name" Enter  # break on function
tmux send-keys -t shared "cl 1" Enter             # clear breakpoint 1
tmux send-keys -t shared "bl" Enter               # list breakpoints

# Evaluate expressions
tmux send-keys -t shared "!x = 5" Enter           # modify variable
tmux send-keys -t shared "interact" Enter         # enter interactive mode
```

### GDB (C/C++/Rust)

```bash
# Navigation
tmux send-keys -t shared "n" Enter      # next
tmux send-keys -t shared "s" Enter      # step
tmux send-keys -t shared "c" Enter      # continue
tmux send-keys -t shared "fin" Enter    # finish function

# Inspection
tmux send-keys -t shared "p variable" Enter
tmux send-keys -t shared "info locals" Enter
tmux send-keys -t shared "bt" Enter
tmux send-keys -t shared "frame 2" Enter

# Breakpoints
tmux send-keys -t shared "b main.c:42" Enter
tmux send-keys -t shared "watch variable" Enter   # watchpoint
tmux send-keys -t shared "info break" Enter
```

### Node.js

```bash
tmux send-keys -t shared "node inspect script.js" Enter
tmux send-keys -t shared "n" Enter                # next
tmux send-keys -t shared "repl" Enter             # enter repl to inspect
tmux send-keys -t shared "exec('variable')" Enter
```

## REPLs

Interactive exploration and testing.

### Python

```bash
tmux send-keys -t shared "python" Enter
tmux send-keys -t shared "x = [1, 2, 3]" Enter
tmux send-keys -t shared "sum(x)" Enter
tmux send-keys -t shared "import module; dir(module)" Enter
tmux capture-pane -t shared -p  # see the results
```

### Clojure

```bash
tmux send-keys -t shared "clj" Enter
tmux send-keys -t shared "(def data {:a 1 :b 2})" Enter
tmux send-keys -t shared "(reduce + (vals data))" Enter
tmux send-keys -t shared "(doc function-name)" Enter
```

### GHCi (Haskell)

```bash
tmux send-keys -t shared "ghci" Enter
tmux send-keys -t shared ":load Module.hs" Enter
tmux send-keys -t shared ":t expression" Enter    # type info
tmux send-keys -t shared ":i TypeName" Enter      # info
```

### Node

```bash
tmux send-keys -t shared "node" Enter
tmux send-keys -t shared "const x = require('./module')" Enter
tmux send-keys -t shared "Object.keys(x)" Enter
```

## TUI applications

### Neovim

```bash
# Test a keybinding
tmux send-keys -t shared Space g g      # leader + gg
sleep 0.3
tmux capture-pane -t shared -p

# Run a command
tmux send-keys -t shared ":Telescope find_files" Enter

# Check LSP status
tmux send-keys -t shared ":LspInfo" Enter

# Exit
tmux send-keys -t shared Escape ":qa!" Enter
```

### Lazygit

```bash
# Navigate panels (1-5)
tmux send-keys -t shared 2              # files panel

# Stage/unstage
tmux send-keys -t shared a              # stage all
tmux send-keys -t shared Space          # toggle file

# Commit
tmux send-keys -t shared c
tmux send-keys -t shared "commit message" Enter

# Exit
tmux send-keys -t shared q
```

### htop/btop

```bash
# Sort by CPU
tmux send-keys -t shared P

# Sort by memory
tmux send-keys -t shared M

# Kill process (select first, then)
tmux send-keys -t shared k

# Search
tmux send-keys -t shared /
tmux send-keys -t shared "process_name" Enter
```

## Key reference

```bash
# Special keys
Enter, Escape, Space, Tab, BSpace
Up, Down, Left, Right
Home, End, PgUp, PgDn

# Modifiers
C-x     # Ctrl+X
M-x     # Alt+X
C-M-x   # Ctrl+Alt+X

# Function keys
F1, F2, ... F12
```

## Useful tmux commands

```bash
# Session management
tmux new -s shared                      # create session
tmux list-sessions                      # list sessions
tmux kill-session -t shared             # kill session

# Pane info
tmux display -t shared -p '#{pane_width}x#{pane_height}'

# Capture options
tmux capture-pane -t shared -p          # basic capture
tmux capture-pane -t shared -p -S -50   # include 50 lines scrollback

# Force refresh if garbled
tmux send-keys -t shared C-l
```

## Limitations

- Text-only capture (no colors unless using `-e` flag)
- Need small delays (~0.3s) between send and capture
- Complex Unicode may not render cleanly
- Terminal apps only, not GUI applications

## Quick start

1. User starts tmux: `tmux new -s shared`
2. User launches the app inside tmux
3. Claude captures: `tmux capture-pane -t shared -p`
4. Claude sends keys: `tmux send-keys -t shared "command" Enter`
5. Repeat: capture → analyze → send → capture
