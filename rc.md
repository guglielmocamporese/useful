# Useful RCs
Put the rc files in your home.

### .profile
```bash
#!/bin/bash

# Aliases
alias ll="ls -l"

# PS1 format
c_col="\e[38;5;51m"
c_bold="\e[1m"
c_off="\e[0m"
export cluster_name="\u@\h"
ps1="${c_col}[${c_col}${cluster_name}: ${c_col}\W${c_col}]\$ "
export PS1="${c_bold}${ps1}${c_off}"
```

### .screenrc
```bash
#!/bin/bash

shell -$SHELL 
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
syntax enable
```

### .zshrc
```bash
#!/bin/bash

# Alias
alias ll="ls -l"

# Colors
export TERM="xterm-256color"
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced

# Formatting parameters
BOLD="%B"; EBOLD="%b"
COL="%F{203}"; ECOL="%f"

# Format prompt
export PROMPT="${BOLD}${COL}[%n@local: %~]$ ${ECOL}${ECOL}${EBOLD}"
```

