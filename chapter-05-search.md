# Chapter 5: Search and Replace (Like It's 1976)

*"I've been trying to replace all occurrences of 'foo' with 'bar' for an hour. I've replaced my will to live with despair instead."*  
— Stack Overflow, 2020

## The Search Basics (Finding Your Misery)

In Emacs, you press `C-s` for incremental search. It highlights matches as you type, `C-s` again goes to the next match, and `C-r` searches backward. Intuitive. Interactive. Modern.

In Vim, you press `/` and then type your search term, but nothing happens until you press Enter. It's like Vim is saying, "Are you SURE you want to search? Really sure? Okay, ENTER to confirm."

### Forward Search
- `/pattern` - Search forward
- `n` - Next match
- `N` - Previous match (because lowercase-n for next, uppercase-N for previous makes total sense)

### Backward Search
- `?pattern` - Search backward (because ? is like / but backwards, geometrically speaking?)
- `n` - Previous match when searching backward (wait, what?)
- `N` - Next match when searching backward (I'm confused)

Yes, `n` and `N` swap meanings depending on which direction you initially searched. It's like Vim is actively trying to confuse you.

## The Case Sensitivity Dance

Want to search case-insensitively? Here are your options:

- `/\cpattern` - Force case-insensitive for this search
- `/\Cpattern` - Force case-sensitive for this search
- `:set ignorecase` - Ignore case for all searches
- `:set smartcase` - Ignore case unless you type an uppercase letter

Smartcase is actually clever: searching for "foo" finds "foo", "Foo", and "FOO", but searching for "Foo" only finds "Foo" and "FOO". Point to Vim, reluctantly.

In Emacs? `M-c` toggles case sensitivity during search. One key combo. While searching. Like a civilized editor.

## Search and Replace (The Substitute Command of Doom)

Here it is, the command that launches a thousand Google searches:

```
:%s/old/new/g
```

Let's break this down:
- `:` - Enter command mode
- `%` - Apply to entire file (not just current line)
- `s` - Substitute command
- `/old/new/` - Replace "old" with "new"
- `g` - Global (all occurrences on each line)

### The Flags (Because Simple Isn't Vim's Style)

- `g` - Global (all occurrences on a line)
- `c` - Confirm each replacement
- `i` - Case insensitive
- `I` - Case sensitive (override 'ignorecase' setting)

Without `g`, it only replaces the first occurrence on each line. Because that's definitely what you want by default. Said no one ever.

### Range Specifications (Making It Complicated)

- `:s/old/new/` - Current line only
- `:%s/old/new/` - Entire file
- `:5,10s/old/new/` - Lines 5 to 10
- `:'<,'>s/old/new/` - Visual selection (appears automatically when you press `:` in VISUAL mode)
- `:.,$s/old/new/` - From current line to end of file
- `:.,+5s/old/new/` - Current line and next 5 lines

In Emacs? `M-%` for query-replace, `!` to replace all. Done. No need for a PhD in range specifications.

## Regular Expressions (Now With Extra Backslashes!)

Vim uses its own regex flavor because standard regex wasn't confusing enough:

### Basic Regex (Where Nothing Is What It Seems)

- `.` - Any character (okay, this is standard)
- `*` - Zero or more of the previous character (standard)
- `\+` - One or more (needs backslash, because reasons)
- `\?` - Zero or one (also needs backslash)
- `\{n,m}` - Between n and m occurrences (backslashes everywhere!)

### Magic Mode (Vim's Regex Identity Crisis)

Vim has four levels of regex "magic":
- `\v` - "Very magic" (more like normal regex)
- `\m` - "Magic" (default, somewhat magical)
- `\M` - "Nomagic" (less magic)
- `\V` - "Very nomagic" (literally literal)

Example of the same pattern in different magic modes:
- Very magic: `/\v(foo|bar)+`
- Magic: `/\(foo\|bar\)\+`
- Nomagic: `/\(foo\|bar\)\+`
- Very nomagic: `/(foo|bar)+` (treats most things as literal)

Why four modes? Because Vim couldn't decide if it wanted to be like Perl, sed, or something entirely different, so it chose "all of the above."

## Practical Search and Replace Examples

### The Simple Replace (That's Never Simple)

**Task**: Replace all "foo" with "bar"

**Emacs**: `M-% foo RET bar RET !`

**Vim**: `:%s/foo/bar/g`

Vim's is shorter! Point to Vim! Until you forget the `g` and wonder why only the first "foo" on each line was replaced.

### The Confirmation Replace

**Task**: Replace "foo" with "bar" but confirm each one

**Vim**: `:%s/foo/bar/gc`

When you add `c`, Vim asks for each match:
- `y` - Yes, replace
- `n` - No, skip
- `a` - All remaining
- `q` - Quit
- `l` - Last (replace this one and quit)
- `^E` - Scroll up
- `^Y` - Scroll down

Seven options for a yes/no question. Classic Vim.

### The Word Boundary Replace

**Task**: Replace "foo" but not "foobar"

**Vim**: `:%s/\<foo\>/bar/g`

`\<` and `\>` are word boundaries. Not `\b` like in every other regex implementation. Because standards are for conformists.

### The Capture Group Replace

**Task**: Swap "first,last" to "last, first"

**Vim**: `:%s/\(\w\+\),\(\w\+\)/\2, \1/g`

Notice the escaped parentheses? In "very magic" mode it would be:
`:%s/\v(\w+),(\w+)/\2, \1/g`

Much better! Only took an extra magic incantation.

## Search History (For Your Previous Mistakes)

- `/` then `↑` - Previous searches
- `/` then `↓` - Next searches
- `q/` - Open search history in a window (exit with `:q`, naturally)

That last one opens your search history in a Vim buffer. You can edit your previous searches in Vim. It's Vim all the way down.

## The Grep Integration (When Internal Search Isn't Enough)

- `:grep pattern files` - Search in files using external grep
- `:vimgrep pattern files` - Search using Vim's internal grep
- `:cn` - Next match
- `:cp` - Previous match
- `:copen` - Open quickfix window with all matches

Why both `grep` and `vimgrep`? Because Vim loves giving you options, even when you don't want them.

## Search Tips and Tricks

### The Star and Hash

- `*` - Search forward for word under cursor
- `#` - Search backward for word under cursor

These are genuinely useful. In Emacs, you'd need to... actually, this is one thing Vim does better. Dammit.

### The Search Highlighting Toggle

- `:set hlsearch` - Highlight all search matches
- `:nohlsearch` or `:noh` - Turn off highlighting temporarily
- `:set nohlsearch` - Turn off highlighting permanently

The highlighting persists after you're done searching, glowing at you like a neon sign saying "YOU SEARCHED FOR THIS." You'll be typing `:noh` constantly.

## Real-World Search and Replace Disasters

### The "Forgot the g Flag" Classic
```vim
:%s/TODO/DONE/
```
Replaces only the first TODO on each line. Your task list is now partially DONE. Just like your patience.

### The "Overzealous Regex" Catastrophe
```vim
:%s/.*/@/g
```
You meant to replace something specific but forgot to escape a character. Your entire file is now just "@" symbols.

### The "Case Sensitivity Surprise"
```vim
:%s/function/method/g
```
Also replaces "Function", "FUNCTION", and "FuNcTiOn" if ignorecase is set. Your code is now full of "method" in random cases.

## The Search and Replace Workflow

1. Type `/%s/old/new/g` (forget the `:`)
2. Wonder why it's searching for "%s/old/new/g"
3. Press `ESC`
4. Type `:%s/old/new/g`
5. Realize you wanted to confirm each change
6. Undo with `u`
7. Type `:%s/old/new/gc`
8. Press `y` 50 times
9. Realize you could have pressed `a`
10. Feel deep regret

## Survival Guide

| Task | Command | What You'll Actually Do |
|------|---------|------------------------|
| Search forward | `/pattern` | This works fine |
| Search and replace all | `:%s/old/new/g` | Google it every time |
| Search word under cursor | `*` | Actually use this |
| Turn off highlighting | `:noh` | Map this to a key immediately |
| Search and replace with confirm | `:%s/old/new/gc` | Press `a` after the first match |

## Next Up

Now that you can find and replace text (with great difficulty), let's learn about Vim's window management. Because one Vim window isn't confusing enough—let's have multiple!

---

*Next Chapter: [Buffers, Windows, and Tabs (Oh My!)](chapter-06-buffers.md)*

*Remember: The hardest part of search and replace in Vim isn't the regex—it's remembering whether you need `:%s`, `:s`, or `:'<,'>s`, and whether you need `/g`, `/gc`, or `/gi`. In Emacs, it's just `M-%`. But who's counting?*