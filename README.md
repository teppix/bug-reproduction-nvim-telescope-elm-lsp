# Reproduction of a bug in dev tooling.

## Tested configuration

- neovim 0.10.0 with:
  - telescope.nvim @ bfcc7d5
  - elm-language-server 2.8.0

## Problem

`:Telescope lsp_document_symbols` reports an error in `Bad.elm` but not in `Good.elm`

```
Error executing vim.schedule lua callback: ...share/nvim/lazy/telescope.nvim/lua/telescope/pickers.lua:1474: Cursor position outside buffer                             
stack traceback:                                                                                                                                                        
        [C]: in function 'nvim_win_set_cursor'                                                                                                                          
        ...share/nvim/lazy/telescope.nvim/lua/telescope/pickers.lua:1474: in function ''                                                                                
        vim/_editor.lua: in function <vim/_editor.lua:0>
```

## Analysis

According to [this comment](https://github.com/nvim-telescope/telescope.nvim/issues/3163#issuecomment-2167678288)
this might be caused by newline characters in the data passed to telescope.

This seems to uniquely happen for certain syntactic constructs, which leads me to believe that elm-language-server is the root issue.

The files `src/Good.elm` and `src/Bad.elm` illustrates a working and a non-working example.
