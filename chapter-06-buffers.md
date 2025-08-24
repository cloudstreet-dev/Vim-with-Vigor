# Chapter 6: Buffers, Windows, and Tabs (Oh My!)

*"Vim has buffers, windows, and tabs. They don't mean what you think they mean. Nothing in Vim means what you think it means."*  
— Vim Survivor, 2022

## The Three-Layer Confusion Cake

In normal applications:
- **Tab**: A tab
- **Window**: A window
- **Buffer**: What's a buffer?

In Vim:
- **Buffer**: An open file (might not be visible)
- **Window**: A viewport into a buffer (not a window)
- **Tab**: A collection of windows (definitely not a tab)

It's like Vim deliberately chose the most confusing terminology possible. "What should we call the thing that holds multiple viewports?" "How about 'tab'?" "But that's not what tabs are—" "PERFECT."

## Buffers (The Invisible Files)

In Emacs, when you open a file, you see it. Revolutionary, I know. In Vim, when you open a file, it goes into a buffer. You might see it. You might not. It's Schrödinger's file.

### Buffer Commands (The Basics)

- `:e filename` - Edit file (open in current window)
- `:ls` or `:buffers` - List all buffers
- `:b {number}` - Go to buffer number
- `:b {partial_name}` - Go to buffer by partial name match
- `:bn` - Next buffer
- `:bp` - Previous buffer
- `:bd` - Delete buffer (close file)
- `:bufdo {command}` - Run command on all buffers

### The Buffer List (A Mystery Wrapped in an Enigma)

When you type `:ls`, you see something like:
```
  1      "file1.txt"                    line 1
  2 %a   "file2.txt"                    line 42
  3 #h   "file3.txt"                    line 7
  4  +   "file4.txt"                    line 99
```

What do these symbols mean?
- `%` - Current buffer (the one you're looking at)
- `#` - Alternate buffer (the last one you looked at)
- `a` - Active (loaded and displayed)
- `h` - Hidden (loaded but not displayed)
- `+` - Modified (unsaved changes)
- `-` - Not modifiable
- `=` - Readonly

Seven different states for a file. In Emacs? A file is open or it's not. Modified or it's not. Simple.

### Buffer Navigation (The Random Walk)

- `:b#` or `Ctrl-^` - Toggle between current and alternate buffer
- `:blast` - Go to last buffer
- `:bfirst` - Go to first buffer
- `:ball` - Open all buffers in windows (chaos mode)

That last one is special. It opens every buffer in its own window, creating a grid of files that will make you question your life choices.

## Windows (Not What You Think)

A window in Vim is a viewport into a buffer. You can have multiple windows showing the same buffer, or different buffers. It's like having multiple monitors, except they're all crammed into your terminal.

### Splitting Windows (Dividing Your Confusion)

- `:split` or `:sp` - Horizontal split
- `:vsplit` or `:vsp` - Vertical split
- `Ctrl-w s` - Horizontal split (in NORMAL mode)
- `Ctrl-w v` - Vertical split (in NORMAL mode)
- `:new` - New horizontal window with empty buffer
- `:vnew` - New vertical window with empty buffer

Why both command mode and normal mode versions? Because Vim believes in options. Too many options.

### Window Navigation (The Ctrl-W Dance)

Everything starts with `Ctrl-w`:
- `Ctrl-w h` - Move to left window
- `Ctrl-w j` - Move to down window
- `Ctrl-w k` - Move to up window
- `Ctrl-w l` - Move to right window
- `Ctrl-w w` - Cycle through windows
- `Ctrl-w p` - Go to previous window

Yes, you use hjkl for window navigation too. Consistency! Finally! Except you need to hold Ctrl-w first, so it's actually completely different.

### Window Management (Making It Worse)

- `Ctrl-w =` - Make all windows equal size
- `Ctrl-w _` - Maximize current window height
- `Ctrl-w |` - Maximize current window width
- `Ctrl-w +` - Increase window height
- `Ctrl-w -` - Decrease window height
- `Ctrl-w >` - Increase window width
- `Ctrl-w <` - Decrease window width
- `Ctrl-w q` - Quit current window
- `Ctrl-w o` - Close all other windows ("only")

In Emacs? `C-x 2` to split horizontally, `C-x 3` to split vertically, `C-x 1` to keep only current window, `C-x 0` to close current window. Four commands. Vim has about 20.

### Window Rotation (When You Want to Reorganize Your Chaos)

- `Ctrl-w r` - Rotate windows downward
- `Ctrl-w R` - Rotate windows upward
- `Ctrl-w x` - Exchange with next window
- `Ctrl-w H` - Move window to far left
- `Ctrl-w J` - Move window to bottom
- `Ctrl-w K` - Move window to top
- `Ctrl-w L` - Move window to far right

Because sometimes you split your windows wrong and instead of closing and reopening them like a normal person, you want to play window Tetris.

## Tabs (The Thing That's Not Tabs)

Vim tabs are not tabs. They're collections of windows. It's like having multiple desktops, but calling them "tabs" to confuse everyone who's used a computer in the last 20 years.

### Tab Commands (More Confusion)

- `:tabnew` - Open new tab
- `:tabedit filename` - Open file in new tab
- `:tabclose` - Close current tab
- `:tabonly` - Close all other tabs
- `:tabnext` or `:tabn` or `gt` - Next tab
- `:tabprevious` or `:tabp` or `gT` - Previous tab
- `:tabfirst` - Go to first tab
- `:tablast` - Go to last tab
- `:tabmove {n}` - Move tab to position n

### Tab Navigation (The GT Mystery)

- `gt` - Next tab (mnemonic: "go tab"?)
- `gT` - Previous tab (capital T means backward, obviously)
- `{n}gt` - Go to tab n
- `:tabs` - List all tabs

Why `g`? Because `t` was taken. By the "till" command. Which finds characters. Making perfect sense.

## The Reality of Multiple Files

Here's what actually happens when you work with multiple files in Vim:

1. Open first file: `vim file1.txt`
2. Need to open second file: `:e file2.txt`
3. First file disappears
4. Panic
5. `:ls` to see it's still there
6. `:b1` to go back
7. `:split file2.txt` to see both
8. Window is too small
9. `Ctrl-w =` to make them equal
10. Still too small
11. `:tabclose` (closes the wrong thing)
12. `:q!` and start over

## The Buffer/Window/Tab Confusion Matrix

| You Want | You Think | Vim Thinks | What to Do |
|----------|-----------|------------|------------|
| Open file in new tab | `:tab file.txt` | Create tab containing window showing buffer | `:tabedit file.txt` |
| See two files | Split screen | Create two windows | `:split file2.txt` |
| Close file | `:close` | Close window, not buffer | `:bd` to close buffer |
| Switch files | Switch tabs | Switch buffers | `:bn` or `:b {name}` |

## Hidden Buffers (The Files You Forgot)

Vim keeps buffers around even when you're not looking at them. This is actually useful but also confusing:

- Edit file1.txt
- `:e file2.txt` - file1.txt is now hidden
- Make changes to file2.txt
- `:b file1.txt` - ERROR! No write since last change
- `:w` - Save file2.txt
- `:b file1.txt` - Finally switches

Or use `:set hidden` to allow switching with unsaved changes. Because safety is optional.

## The Practical Multiple File Workflow

What you'll actually do:

1. **The Terminal Tab Method**: Open multiple terminal tabs, run Vim in each
2. **The `:!` Method**: Save file, exit Vim, edit other file, repeat
3. **The Split Chaos Method**: Split windows until text is unreadable
4. **The Buffer Roulette Method**: `:bn` repeatedly until you find your file
5. **The Give Up Method**: Use VS Code with Vim keybindings

## Quick Reference for Sanity

| Task | Command | Easier Alternative |
|------|---------|-------------------|
| List open files | `:ls` | Use IDE |
| Switch to file | `:b filename` | Click on tab |
| Split screen | `:split` | Drag tab |
| Close file | `:bd` | Click X |
| New tab | `:tabnew` | Ctrl+T |

## The Window Management Philosophy

Vim's window management is based on the principle that you should be able to view any buffer in any arrangement at any time. This is powerful if you're editing code across multiple files and need to see relationships.

It's also completely overkill when you just want to edit a config file on a server at 2 AM.

## A Real Scenario

You SSH into a server to edit nginx.conf and need to check the error log:

**The Vim Way**:
1. `vim /etc/nginx/nginx.conf`
2. `:split /var/log/nginx/error.log`
3. `Ctrl-w j` to move between windows
4. Realize log is updating
5. `:e` to reload
6. Repeat step 5 forever

**The Sane Way**:
1. Open two SSH sessions
2. `vim` in one, `tail -f` in the other
3. Use your terminal's native tabs/windows

## Next Up

Now that you're thoroughly confused about buffers, windows, and tabs, let's learn about configuring Vim with the mystical .vimrc file. Because default Vim isn't painful enough—you need to customize your suffering.

---

*Next Chapter: [The .vimrc (Making Peace with the Beast)](chapter-07-vimrc.md)*

*Remember: In Vim, a tab is not a tab, a window is not a window, and a buffer is invisible. It's like the editor was designed by someone who really enjoyed watching people suffer. In Emacs, we just call them buffers and windows, and they work exactly like you'd expect. But where's the fun in that?*