# Chapter 2: Moving Around (Without a Mouse, Like a Barbarian)

*"Why use arrow keys when you can use letters that are nowhere near each other?"*  
— Every Vim Tutorial Ever

## The Home Row Prison

In Emacs, we have beautiful, logical movement commands. `C-f` for forward, `C-b` for back, `C-n` for next line, `C-p` for previous. They make sense! They're mnemonic! They work everywhere!

Vim looked at this and said, "What if we used `h`, `j`, `k`, and `l` instead?"

Why these letters? Because Bill Joy was using an ADM-3A terminal in 1976 that had arrows on these keys. You're not using an ADM-3A terminal. You're probably reading this on a machine that could simulate ten thousand ADM-3A terminals while streaming Netflix. But here we are, pretending our arrow keys don't exist.

## The Sacred Movement Keys

In NORMAL mode (always in NORMAL mode, never forget):

- `h` - Left (because 'h' is to the left... of 'j'?)
- `j` - Down (looks like a down arrow if you squint and have imagination)
- `k` - Up (it's... next to 'j'?)
- `l` - Right (it's to the right of 'k', therefore it must mean right)

### A Memory Aid

Here's how I remember it: I don't. I use the arrow keys like a normal person when nobody's watching.

But if you MUST learn them:
- **H**ome is left (because 'h' sounds like 'home'? Sure.)
- **J**ump down (because J points down, kind of)
- **K**limb up (because K... klimbs? I'm reaching here)
- **L**eap right (because we've run out of reasonable mnemonics)

## Word Movement (Or: How to Jump Around Like a Caffeinated Rabbit)

Vim doesn't just move by character. Oh no, that would be too simple. Here's how to move by word:

- `w` - Move to the beginning of the next word
- `W` - Move to the beginning of the next WORD (yes, there's a difference)
- `b` - Move back to the beginning of the previous word
- `B` - Move back to the beginning of the previous WORD
- `e` - Move to the end of the current/next word
- `E` - Move to the end of the current/next WORD

What's the difference between a "word" and a "WORD"? A "word" is letters, digits, and underscores. A "WORD" is anything that's not whitespace. This distinction will definitely come up never in your actual editing, but Vim is very proud of it.

In Emacs? `M-f` and `M-b`. Forward and backward. Simple. Elegant. No need for a philosophy degree to understand the metaphysical difference between a word and a WORD.

## Line Movement (The Slightly More Reasonable Part)

- `0` - Go to the beginning of the line (the actual beginning)
- `^` - Go to the first non-whitespace character (the beginning that matters)
- `$` - Go to the end of the line (like regex! Finally, something familiar!)

Compare to Emacs:
- `C-a` - Beginning of line
- `C-e` - End of line

Which is easier to remember? I'll wait.

## Screen Movement (Jumping Around Like You're in The Matrix)

- `gg` - Go to the top of the file
- `G` - Go to the bottom of the file
- `{number}G` - Go to line {number}
- `C-f` - Page down (wait, this is the same as Emacs!)
- `C-b` - Page up (also the same as Emacs!)
- `C-d` - Half page down
- `C-u` - Half page up

The half-page movements are actually useful when you're on a tiny terminal on a remote server from 1987. For the rest of us with screens larger than a postcard, full pages work fine.

## The Number Prefix Multiplier Magic

Here's where Vim gets interesting (or annoying, depending on your perspective). You can prefix most commands with a number:

- `5j` - Move down 5 lines
- `3w` - Move forward 3 words
- `10l` - Move right 10 characters (or just hold the right arrow like a normal person)

This is actually pretty clever. In Emacs, you'd use `C-u 5 C-n` to move down 5 lines. In Vim, it's just `5j`. Point to Vim here, I grudgingly admit.

## Finding Characters (The One Thing Vim Does Well)

- `f{char}` - Find the next occurrence of {char} on this line
- `F{char}` - Find the previous occurrence of {char} on this line
- `t{char}` - Go to just before the next {char}
- `T{char}` - Go to just after the previous {char}
- `;` - Repeat the last f/F/t/T movement
- `,` - Repeat the last f/F/t/T movement in the opposite direction

Example: To go to the next comma, type `f,`. To go to the one after that, type `;`. 

This is actually brilliant and I wish Emacs had stolen this idea. There, I said it. Happy now, Vim?

## Searching (When You Give Up on Movement Commands)

- `/pattern` - Search forward for pattern
- `?pattern` - Search backward for pattern
- `n` - Go to next match
- `N` - Go to previous match
- `*` - Search forward for the word under the cursor
- `#` - Search backward for the word under the cursor

The `*` and `#` commands are genuinely useful. In Emacs, you'd have to... well, it's complicated. Point to Vim again.

## Marks (Vim's Bookmarks)

- `m{letter}` - Set a mark at the current position
- `'{letter}` - Jump to the line of the mark
- `` `{letter}`` - Jump to the exact position of the mark

Lowercase letters are local to the buffer, uppercase letters are global. Because of course they are.

## The Practical Movement Cheat Sheet

Here's what you'll actually use:

| I Want To... | Vim Says... | What You'll Actually Do |
|-------------|-------------|------------------------|
| Move left | `h` | Use left arrow |
| Move down | `j` | Use down arrow |
| Go to line start | `0` or `^` | Use Home key |
| Go to line end | `$` | Use End key |
| Go to next word | `w` | Hold Ctrl and use right arrow |
| Find next 'x' | `fx` | Use `/x` and feel bad |
| Go to line 42 | `42G` | This one's actually good |

## A Real-World Scenario

You're on a server. The config file has an error on line 567. Here's how different editors handle it:

**Emacs**: `M-g g 567 RET`  
**Vim**: `567G` or `:567`  
**Nano**: Ctrl+_ (yes, really) then type 567  

Vim wins this round, but only because nano is trying even harder to be obscure.

## Movement Practice Exercises

1. Open Vim (accidentally, as usual)
2. Try to move to the end of the line using `$`
3. Realize you're in INSERT mode
4. Press ESC
5. Try again
6. Accidentally type `$` into your document
7. Press ESC five more times just to be sure
8. Successfully use `$`
9. Feel oddly proud
10. Remember that the End key exists

## The Ultimate Movement Command

When all else fails, remember this powerful Vim movement sequence:

```
:q!
cd ~
emacs
```

This moves you from Vim to a better place.

## Philosophical Note

Vim's movement commands are based on the principle that your fingers should never leave the home row. This made sense when keyboards were mechanical beasts that required significant force to press keys and moving your hands was genuinely tiring.

Today, we have keyboards so sensitive they register keypresses when you breathe on them. We have ergonomic keyboards, split keyboards, keyboards with thumb clusters. We even have keyboards with displays in each key.

But Vim still insists: h, j, k, l. Forever. Because tradition.

## Next Up

Now that you can move around (badly), it's time to learn about Vim's modes. Yes, modes. Plural. Because why should typing text in a text editor be simple?

---

*Next Chapter: [The Modal Madness (Why Can't I Just Type?)](chapter-03-modes.md)*

*Remember: Every Vim user who says they're "faster" with hjkl is secretly using arrow keys when you're not looking.*