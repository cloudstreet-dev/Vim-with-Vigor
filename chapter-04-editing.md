# Chapter 4: Basic Editing (The Hard Way)

*"In Emacs, you edit text. In Vim, you perform text surgery with character-level precision whether you want to or not."*  
— Frustrated Server Administrator, 2023

## The Fundamental Problem

You want to edit text. In Emacs, you:
1. Move to where you want to edit
2. Type, delete, or modify as needed
3. Save

In Vim, you:
1. Make sure you're in NORMAL mode
2. Navigate using arcane movements
3. Choose from 47 different commands to delete text
4. Enter INSERT mode at exactly the right spot
5. Type your text
6. ESC back to NORMAL mode
7. Realize you made a mistake
8. Undo
9. Try again
10. Question your life choices

## Deleting Text (The Many Ways to Destroy)

In Emacs, `C-d` deletes forward, `Backspace` deletes backward. Simple. In Vim? Buckle up:

### Character Deletion
- `x` - Delete character under cursor
- `X` - Delete character before cursor (Shift makes it go backward, sure)
- `r` - Replace single character (doesn't even enter INSERT mode!)
- `s` - Delete character and enter INSERT mode (because `x` then `i` is too much work)

### Word Deletion
- `dw` - Delete to the beginning of next word
- `de` - Delete to the end of current word
- `db` - Delete to the beginning of current word
- `daw` - Delete a word (including surrounding spaces)
- `diw` - Delete inner word (just the word, not spaces)
- `cw` - Delete to end of word and enter INSERT mode
- `ciw` - Change inner word (delete word, enter INSERT mode)

Did you catch the difference between `d` and `c`? One leaves you in NORMAL mode, one puts you in INSERT mode. Because consistency is for the weak.

### Line Deletion
- `dd` - Delete entire line
- `D` - Delete from cursor to end of line
- `C` - Delete from cursor to end of line AND enter INSERT mode
- `cc` - Delete entire line and enter INSERT mode
- `S` - Also delete entire line and enter INSERT mode (why?)

Five ways to delete lines. FIVE. In Emacs? `C-k`. Done.

## The Infamous "Yank" (Copy and Paste)

In the rest of the computing world:
- Copy: Ctrl+C
- Cut: Ctrl+X
- Paste: Ctrl+V

In Vim:
- Copy: `y` (stands for "yank" because "copy" was too obvious)
- Cut: `d` (stands for "delete" but actually cuts)
- Paste: `p` or `P` (paste after or before cursor)

### Yank Variations
- `yy` or `Y` - Yank entire line
- `yw` - Yank word
- `y$` - Yank to end of line
- `y0` - Yank to beginning of line
- `yaw` - Yank a word (with spaces)
- `yiw` - Yank inner word (without spaces)
- `yt,` - Yank till comma
- `yf,` - Yank to and including comma

And this is just the beginning. There are more `y` combinations than there are stars in the sky.

### The Register Madness

Oh, you thought yanking was simple? Vim has registers! Multiple clipboards!

- `"ayy` - Yank line into register 'a'
- `"ap` - Paste from register 'a'
- `"+y` - Yank to system clipboard (sometimes)
- `"*y` - Also yank to system clipboard (but different)
- `:reg` - See all your registers and despair

In Emacs? `M-w` to copy, `C-y` to paste. One clipboard. Like normal people use.

## Undo and Redo (Time Travel for Masochists)

In every normal program:
- Undo: Ctrl+Z
- Redo: Ctrl+Y or Ctrl+Shift+Z

In Vim:
- Undo: `u`
- Redo: `Ctrl+r` (Finally! A Ctrl key command!)

But wait, there's more! Vim has an undo tree. Not an undo list, a TREE. You can branch your undos. There's even a plugin to visualize it. Because apparently, linear undo history is for quitters.

## Text Objects (Vim's One Good Idea)

Okay, I'll admit it. Text objects are actually brilliant. You can operate on semantic units:

- `ci"` - Change inside quotes
- `da(` - Delete around parentheses
- `vi{` - Select inside braces
- `cat` - Change around HTML tag

This is genuinely useful. In Emacs, you'd need to... well, it's complicated. Point to Vim.

### Text Object Cheat Sheet

| Object | Meaning |
|--------|---------|
| `w` | Word |
| `W` | WORD (bigger word) |
| `s` | Sentence |
| `p` | Paragraph |
| `"` | Quoted string |
| `(` or `)` | Parentheses |
| `{` or `}` | Braces |
| `[` or `]` | Brackets |
| `<` or `>` | HTML tags |
| `` ` `` | Backticks |

Combine with `i` (inner) or `a` (around) and an operation (`d`, `c`, `y`, `v`), and you have a powerful system. Credit where it's due.

## The Dot Command (Repetition for the Lazy)

`.` repeats the last change. This is simple but powerful:

1. Delete a word with `dw`
2. Move somewhere else
3. Press `.` to delete another word

In Emacs, you'd record a macro. In Vim, it's just `.`. Another point to Vim, annoyingly.

## Joining Lines (Because Sometimes You Need To)

- `J` - Join current line with next line
- `gJ` - Join without adding a space

Why does `J` add a space but `gJ` doesn't? Because Vim likes to keep you guessing.

## Indentation (The Tab Wars Continue)

- `>>` - Indent line
- `<<` - Outdent line
- `>}` - Indent to next paragraph
- `=` - Auto-indent (sometimes works!)
- `==` - Auto-indent current line
- `gg=G` - Auto-indent entire file (and pray)

In Emacs? `Tab` usually just works. But in Vim, you need to know if you're indenting, outdenting, or auto-indenting, and choose your weapon accordingly.

## Case Manipulation (BECAUSE SOMETIMES you NEED tO ChAnGe CaSe)

- `~` - Toggle case of character
- `g~w` - Toggle case of word
- `gUU` - Uppercase entire line
- `guu` - Lowercase entire line
- `g~$` - Toggle case to end of line

Why is it `g~` and not just a simple command? Because `g` is Vim's junk drawer where they put commands they couldn't fit elsewhere.

## The Real-World Editing Workflow

Here's what actually happens when you edit in Vim:

1. See typo
2. Navigate to typo (using arrow keys, shamefully)
3. Press `x` to delete wrong character
4. Press `i` to enter INSERT mode
5. Type correct character
6. Press `ESC`
7. Realize you could have just used `r`
8. Feel simultaneously smart and stupid
9. Continue editing
10. Make same mistake five minutes later

## Common Editing Patterns

### The "I Need to Change This Word" Dance
```
ciw[type new word]ESC
```

### The "Delete This Line and Start Over" Shuffle
```
ddO[type new line]ESC
```

### The "Copy This Line to Somewhere Else" Tango
```
yyjjp
```
(Yank line, move down twice, paste)

### The "I Give Up, Let Me Just Type" Surrender
```
i[stay in INSERT mode forever]
```

## Survival Commands

Here are the editing commands you'll actually use:

| Task | Command | What You'll Actually Do |
|------|---------|------------------------|
| Delete line | `dd` | This one's actually good |
| Copy line | `yy` | You'll forget and use `dd` then undo |
| Paste | `p` | After three tries to get it right |
| Delete word | `dw` | Select with mouse, press Delete |
| Replace character | `r` | Delete, insert, type, ESC |
| Undo | `u` | Spam this constantly |

## The Editing Philosophy

Vim's editing philosophy is that every possible text manipulation should have its own command. Why press `Backspace` four times when you could press `4X`? Why use `Ctrl+C` and `Ctrl+V` like every other program when you could use `y` and `p`?

It's like Vim was designed by someone who thought "What if we made editing text as complicated as possible, then claimed it was more efficient?"

## Next Up

Now that you can (barely) edit text, let's learn about search and replace. Because finding and replacing text shouldn't be simple. That would be too easy.

---

*Next Chapter: [Search and Replace (Like It's 1976)](chapter-05-search.md)*

*Remember: In Vim, there are 17 ways to delete a word, but only one way to maintain your sanity: switching to Emacs.*