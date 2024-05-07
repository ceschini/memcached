# TMUX Cheatsheet

TMUX is a Terminal MUltipleXer that helps you manage different terminal processes on a session based environment.
It allows for command line interfaces to remain active even if our remote connection breaks up. This is vital in order to
keep vital processes running, like model training and testing, for example.

## TMUX Key Commands and Concepts

Here we will list some of the most usefull commands extracted from the awesome [Tmux Cheat Sheet](https://tmuxcheatsheet.com/){:target=”_blank”} website, with a brief explanation of the three key concepts of the system.

### Terminology

- **$** - commands typed on the shell
- `ctrl + keys` - key combinations pressed inside tmux
- `:` - commands typed on tmux command mode

### Sessions

Sessions is our main process, where tmux starts out by creating a asynchronous process and opening our terminal to its first and main window.
By creating a session we can organize and group its windows and processes by a meaningful name. Here are the key commands.

#### Start a new session

- $ tmux
- $ tmux new
- $ tmux new-session
- `:new`

##### With a name

- $ tmux new -s mysession
- `:new -s mysession`

#### Show all sessions

- $ tmux ls
- $ tmux list-sessions
- `ctrl + b` `s`

#### Detach from session

- `ctrl + b` `d`

#### Attach to last session

- $ tmux a

##### Attach to named session

- $ tmux a -t myssesion

### Windows

Inside a session, we can manage multiple windows. Windows are terminal sub-processes that have its own interface, where we can name it, switch between them, and so on.

- **Create window**: `ctrl + b` `c`
- **Rename current window**: `ctrl + b` `,`
- **Close current window**: `ctrl + b` `&`
- **List windows**: `ctrl + b` `w`
- **Previous window**: `ctrl + b` `p`
- **Next window**: `ctrl + b` `n`
- **Toggle last active window**: `ctrl + b` `l`

### Panes

Each window can be broken down on multiple panes. When we do this, the window will be split into multiple processes, visible over a single window interface.

- **Split pane with horizontal layout**: `ctrl + b` `%`
- **split pane with vertical layout**: `ctrl + b` `"`
- **Toggle last active pane**: `ctrl + b` `;`
- **Switch to the pane on direction**:
  - `ctrl + b` &uarr;
  - `ctrl + b` &darr;
  - `ctrl + b` &rarr;
  - `ctrl + b` &larr;
- **Toggle between pane layouts**: `ctrl + b` `spacebar`
- **Switch to next pane**: `ctrl + b` `o`
- **Convert pane into window**: `ctrl + b` `!`
- **Close current pane**: `ctrl + b` `x`

### Copy mode

By default, tmux have a fixed window size, and mouse scroll commands will circle through the session command history. In order to visualize and/or capture previous outputs, it is required to enter copy mode. Here are the key commands.

- **Enter copy mode**: `ctrl + b` `[`
- **Enter copy mode and scroll up**: `ctrl + b` `PgUp`
- **Quit mode**: `q`
- **Go to top line**: `g`
- **Go to bottom line**: `G`
- **Scroll up**: &uarr;
- **Scroll down**: &darr;
- **Move cursor**:
  - **up**: `k`
  - **down**: `j`
  - **left**: `h`
  - **right**: `l`
- **search forward**: `/`
- **search backward**: `?`
- **next keyword occurance**: `n`
- **previous keyword occurance**: `N`
- **start selection**: `spacebar`
- **clear selection**: `Esc`
- **Copy selection**: `Enter`
- **paste contents of buffer_0**: `ctrl + b` `]`

*For buffer manipulation, check [tmux cheatsheet page](https://tmuxcheatsheet.com/)*

### Misc

- **Enter command mode**: `ctrl + b` `:`
- **Enable mouse mode**: `: set mouse on`
- **List key bindings**:
  - $ tmux list-keys
  - `: list-keys`
  - `ctrl + b` `?`
- **Show every session, window, pane, etc**: $ tmux info
