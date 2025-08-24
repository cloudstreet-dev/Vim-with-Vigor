# Chapter 8: Survival Commands for Server Life

*"I don't need to be good at Vim. I just need to edit this config file and get out alive."*  
— Every System Administrator Ever

## The Reality Check

You're SSH'd into a production server. It's 2 AM. Something's broken. You need to edit a configuration file. There's no Emacs. There's no nano. There's no VS Code remote extension. There's only Vim.

This chapter is for those moments. Not Vim mastery. Just survival.

## The Server Survival Kit

Here are the only commands you actually need to know:

```bash
# Open file
vim /etc/nginx/nginx.conf

# Navigate to line 42 (from error message)
:42 [Enter]

# Fix the problem
i [make your edit] ESC

# Save and exit
:wq [Enter]

# Or if you screwed up
:q! [Enter]
```

That's it. That's 90% of server Vim usage.

## The "Quick Fix" Workflow

### Scenario 1: Fix a Typo

```bash
vim config.yml
/typo [Enter]          # Search for the typo
cwcorrect [ESC]        # Change word to "correct"
:wq [Enter]            # Save and exit
```

### Scenario 2: Comment Out a Line

```bash
vim app.conf
:15 [Enter]            # Go to line 15
I# [ESC]               # Insert '#' at line beginning
:wq [Enter]            # Save and exit
```

### Scenario 3: Add a Line to a File

```bash
vim ~/.bashrc
G                      # Go to end of file
o                      # Open new line below
export PATH=$PATH:/usr/local/bin [ESC]
:wq [Enter]
```

### Scenario 4: Emergency Config Rollback

```bash
vim broken.conf
:%d [Enter]            # Delete everything
:r working.conf.backup [Enter]  # Read in backup file
:wq [Enter]            # Save and exit
```

## The Copy-Paste Between Files Dance

You need to copy configuration from one file to another:

```bash
# Open first file
vim source.conf
/section_you_need [Enter]
V                      # Visual line mode
]p                     # Select to end of paragraph
y                      # Yank (copy)
:e target.conf [Enter] # Open second file
G                      # Go to end
p                      # Paste
:wq [Enter]            # Save and exit
```

## The "View Without Breaking" Mode

Sometimes you just need to look:

```bash
vim -R /etc/important.conf    # Readonly mode
# or
view /etc/important.conf       # Same thing
```

You can still edit in readonly mode (because Vim), but it'll warn you when you try to save.

## Log File Investigation

```bash
vim /var/log/application.log
G                      # Go to end (latest entries)
?ERROR [Enter]         # Search backwards for ERROR
n                      # Previous ERROR
n                      # Previous ERROR
:q [Enter]             # Exit without saving
```

Pro tip: Use `less` or `tail` for logs. Vim is overkill.

## The sudo Save Trick

You opened a system file without sudo. You made changes. Now you can't save:

```vim
:w !sudo tee %
```

What this does:
- `:w !` - Write to a command
- `sudo tee` - Run tee with sudo
- `%` - Current filename

It's magic. Don't question it. Just use it.

## Emergency Vim Settings

No .vimrc on the server? Set these temporarily:

```vim
:set number            # See line numbers
:set paste            # Paste without indent madness
:set noai             # Turn off autoindent
:syntax on            # Syntax highlighting
```

These only last for the current session. Which is fine because you're leaving ASAP.

## The "Something Went Wrong" Commands

### You're Lost and Confused
```
ESC ESC ESC           # Return to normal mode
:q! [Enter]           # Exit without saving
```

### You Broke the File
```
u                     # Undo
u                     # Undo more
u                     # Keep undoing
:q! [Enter]           # Give up and exit
```

### Vim Is Frozen
```
Ctrl+Q                # Unfreeze (you probably hit Ctrl+S)
```

### You Opened Binary/Huge File
```
:q! [Enter]           # Exit immediately
# Or if that doesn't work
Ctrl+Z                # Suspend Vim
kill %1               # Kill the suspended job
```

## Common Server Files and How to Edit Them

### nginx.conf
```bash
vim /etc/nginx/nginx.conf
/server_name [Enter]   # Find server block
cwmydomain.com [ESC]   # Change domain
:wq
nginx -t               # Test configuration
systemctl reload nginx # Apply changes
```

### SSH authorized_keys
```bash
vim ~/.ssh/authorized_keys
G                      # Go to end
o                      # New line
[paste your key] [ESC]
:wq
```

### Crontab (The Special Case)
```bash
crontab -e             # Opens in default editor (usually vi/vim)
G                      # Go to end
o                      # New line
0 2 * * * /backup.sh   # Your cron job
[ESC]
:wq
```

### /etc/hosts
```bash
sudo vim /etc/hosts
G
o
192.168.1.100 myserver.local [ESC]
:wq
```

## The Different Vim Versions Problem

Different servers have different Vims:

- `vi` - Might be actual vi (even worse than Vim)
- `vim` - Usually Vim, but what version?
- `vim.tiny` - Minimal Vim (Debian/Ubuntu)
- `vim.basic` - Basic Vim
- `nvim` - Neovim (if you're lucky)

When in doubt:
```bash
which vim
vim --version
```

## Vim Alternatives on Servers

Before surrendering to Vim, check for:

```bash
which nano            # Simpler editor
which emacs          # The promised land
which ed             # Just kidding, don't use ed
which sed -i         # Edit without an editor
which echo >>        # Append without an editor
```

## The "Edit Without Vim" Tricks

### Append to File
```bash
echo "new line" >> file.txt
```

### Replace in File
```bash
sed -i 's/old/new/g' file.txt
```

### Create Simple File
```bash
cat > config.txt << EOF
line 1
line 2
EOF
```

### View Without Editing
```bash
cat file.txt
less file.txt
tail -n 20 file.txt
head -n 20 file.txt
```

## The Real-World Server Workflow

1. Try to avoid editing files on servers
2. Use configuration management (Ansible, etc.)
3. If you must edit, use nano if available
4. If only Vim is available, keep it simple:
   - Open file
   - Make minimal changes
   - Save and exit
   - Test your changes
   - Document what you did

## The Server Vim Mantras

- "I don't need to be good at Vim"
- "This is temporary"
- "Just fix and exit"
- "Don't try anything fancy"
- "ESC :wq is my friend"
- "When in doubt, :q!"

## Emergency Command Reference Card

Print this and tape it to your monitor:

```
SURVIVAL COMMANDS
=================
vim file           - Open file
i                  - Start typing
ESC                - Stop typing
:w                 - Save
:q                 - Quit
:wq                - Save and quit
:q!                - Quit without saving
u                  - Undo
/text              - Search for text
n                  - Next search result
:42                - Go to line 42
dd                 - Delete current line
yy                 - Copy current line
p                  - Paste
```

## The Honest Truth

You don't need to be a Vim expert to be a good developer or sysadmin. You just need to know enough to:

1. Open a file
2. Make a change
3. Save the file
4. Exit Vim

Everything else is optional. Vim evangelists will tell you about the efficiency gains, the powerful commands, the editing philosophy. But when you're on a production server at 2 AM, you don't care about philosophy. You care about fixing the problem and going back to bed.

## Next Up

You've survived this long. You know enough Vim to handle server emergencies. Now, let's talk about life after Vim—returning to the warm embrace of Emacs or modern editors.

---

*Next Chapter: [Epilogue: When You Can Finally Use Emacs Again](chapter-09-epilogue.md)*

*Remember: Using Vim on a server doesn't make you a Vim user. It makes you a survivor. There's no shame in immediately installing nano after gaining sudo access.*