# VIM Cheat Sheet

This is just my personal VIM Cheat Sheet

> All these things are basically just stolen from other webpages. Mostly it's gonna be [vim.wikia.com](http://vim.wikia.com/wiki)

## Delete all lines containing a pattern

For example, to delete all lines containing "profile" (remove the /d to show the lines that the command will delete):

```
:g/profile/d
```

## Open command-line history 

Type `:` or `/` to start entering a command or search, then press the 'cedit' key (default is `Ctrl-f` `:help 'cedit'`).
The advantage of the command-line window is that you can use all Vim's editing power, including searching with `/` in normal mode, or using whole-line completion (`:help compl-whole-line`) in insert mode. After editing a command, you can:

Press Enter to execute the current line (and close the command-line window); or
Press Ctrl-c twice to close the command-line window (cancel).
