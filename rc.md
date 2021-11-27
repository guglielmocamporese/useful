# Useful RCs

### .screenrc
```bash
#!/bin/bash

# Formatting parameters
CGREEN="\e[32m"
CEND="\e[0m"
CBOLD="\e[1m"
CBLUE="\e[34m"

# Format prompt
PS1="${CBOLD}${CGREEN}\u@\h${CEND}${CEND}:${CBOLD}${CBLUE}\w${CEND}${CEND}$ "

termcapinfo xterm|xterms|xs|rxvt ti@:te@

# Erase background with current bg color
defbce "on"

# Cache 30000 lines for scroll back
defscrollback 30000

hardstatus alwayslastline
# Very nice tabbed colored hardstatus line
hardstatus string "%{= Kr}%{= Kd}%-w%{= Kr}[%{= KW}${CBOLD}%S${CEND}%{= Kr}]%{= Kd}%+w %-= %{KG} %H%{KW}|%{KY}%101`%{KW}|%D %M %d %Y%{= Kc} %C%A%{-}"
altscreen on
```

### .vimrc
```bash
set number                                                                                                              
set colorcolumn=120
                                                                                                                        
filetype plugin indent on                                                                                               
" show existing tab with 4 spaces width                                                                                 
set tabstop=4                                                                                                           
" when indenting with '>', use 4 spaces width                                                                           
set shiftwidth=4                                                                                                        
" On pressing tab, insert 4 spaces                                                                                      
set expandtab  
```
