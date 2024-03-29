# VIM Cheat Sheet

These are just a few vim function that i use regularly.

> All these things are basically just stolen from other webpages. Mostly it's gonna be [vim.wikia.com](http://vim.wikia.com/wiki)

## Settings for Python:

```vim
set textwidth=79  " lines longer than 79 columns will be broken  
set shiftwidth=4  " operation >> indents 4 columns; << unindents 4 columns  
set tabstop=4     " a hard TAB displays as 4 columns  
set expandtab     " insert spaces when hitting TABs  
set softtabstop=4 " insert/delete 4 spaces when hitting a TAB/BACKSPACE  
set shiftround    " round indent to multiple of 'shiftwidth'  
set autoindent    " align the new line indent with the previous line  
```

## Overview

![VIM Cheat Cheat](https://i.imgur.com/YLInLlY.png)

## Sort lines 

`:{range}sort u`
[Check this](https://vim.fandom.com/wiki/Sort_lines)

## Count number of matches of a pattern

`:%s/pattern//gn`
[Check this](http://vim.wikia.com/wiki/Count_number_of_matches_of_a_pattern)

## Multiline Edit (like Sublime)

[Check this](https://stackoverflow.com/a/11790464)

## Highlight current line

`:set cursorline`
[cursorline](http://vim.wikia.com/wiki/Highlight_current_line)

## Get help inside vim

`:help`
[help](http://vim.wikia.com/wiki/Help)

## Folding

[Folding](https://vim.fandom.com/wiki/Folding)

### Folding Shortcuts

| Shortcut | Function |
| -------- | -------- |
| `zc`     | Close Fold |
| `zo`     | Open Fold |
| `za`     | Toggle current fold |

### Disable Folding

`:set nofoldenable` in command mode, or add `set nofoldenable` to your `.vimrc`

## Add new lines

`o` `N o` open a new line below the current line, append text (N times)  
`O`	`N O`	open a new line above the current line, append text (N times)

## Delete all lines containing a pattern

For example, to delete all lines containing "pattern" (remove the /d to show the lines that the command will delete):

```
:g/pattern/d
```

## Open command-line history 

Type `:` or `/` to start entering a command or search, then press the 'cedit' key (default is `Ctrl-f` `:help 'cedit'`).
The advantage of the command-line window is that you can use all Vim's editing power, including searching with `/` in normal mode, or using whole-line completion (`:help compl-whole-line`) in insert mode. After editing a command, you can:

Press Enter to execute the current line (and close the command-line window); or
Press Ctrl-c twice to close the command-line window (cancel).

## Search inside files with `:vimgrep`

### Recursive Search

You can use `**` in the file pattern to search recursively. For example, to search for all lines containing "dostuff()" in all .c files in the parent directory and all its subdirectories, use:

```
:vimgrep /dostuff()/j ../**/*.c
```

## Reuse something that you wanted to replace

Example Text:

```
foo bara blupp bla blupp barb foo
```

Comannds:

| Command | Result |
| ------- | ------ |
| `.s/.*\(bar\w\).*/\1/` | `barb` |
| `.s/.*\(bar\w\).*\(bar\w\).*/\1/` | `bara` |
| `.s/.*\(bar\w\).*\(bar\w\).*/\2/` | `barb` |

## Pretty format json file in vim

`:%! python -m json.tool`

or to reverse in pure vimscript:

`%delete | 0put =json_encode(json_decode(@@))`

## How to avoid closing a complete Browser Tab when using `Ctrl-w`

<iframe src="https://giphy.com/embed/lKrrumbdd7LY4" width="480" height="272" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/might-trollface-lKrrumbdd7LY4">via GIPHY</a></p>
