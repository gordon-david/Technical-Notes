# Personal Vim Rundown
### Fuzzy File Search
Finding Files:
-   search down into subfolders, provides tab-completion on file related searches (:find {}, fuzzy with regex => given a list)
    -   `:set path=**`
-   display all matching files when we tab complete
    -   :set wildmenu

### Tag Jumping
vim knows how to read tags files.

create tags file first (may need ctags ,universal ctags)
:   command! MakeTags !ctags -R .

Now we can:
-   c-\] for jump to tag under curser
-   g-c-\] for ambiguous tags
-   c-t to jump back up tag stack

doensn\'t give a visual list of tags

### Autocomplete
reads from tags file if one exists.
-   ^xn^ for just this file
-   ^xf^ for files names, works with set path tricks
-   ^n^ for anything specified by \'complete\' option

Now we can:
-   ^n^ and ^p^ to go back and forth in suggestion list

### File Browsing
### Snippets
### Build Integration
### Windows
Navigation
:   c-w h/j/k/l/w

new windows
:   c-w v/s

close window
:   c-w q

### Integrated Terminal
- Open -> :term
- close -> <CTRL-\><CTRL-n>

### Development Environments
*Syntax Highlighting*: vim-polyglot
*Linting*: ALE (Asyncronous Lint Engine)
*Completion*:
- vim-lsp
- coc.nvim
    - :CocInstall coc-json coc-tsserver coc-java ...etc 
## Plugins
### VimWiki
- <leader>i : opens index
- <tab> : next link
- <shift><tab> : prev link
- <enter> : follow link
- <backspace> : up a link

### Taskwiki 'tools-life/taskwiki'
manage tasks and todos, specifically a vim interface for taskwarrior
**Dependencies**:
- Vimwiki: github.com/vimwiki/vimwiki
- Taskwarrior: sudo dnf install task
- tasklib: sudo pip# install --upgrade -r (github.com/tools-life/taskwarrior#requirements.txt)

### Markdown Drawer
`:MarkDrawer` -> opens
`o` -> nav to header in file
`D` -> mark section for cut
`p` -> paste marked section below current
`+` -> remove #
`-` -> decrease header, add #
### Markdown Folding
`za` -> toggle fold region

