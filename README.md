## tmux configuration

```bash
brew install tmux
brew install reattach-to-user-namespace
```

参考`/usr/local/share/tmux/examples/vim-keys.conf`，做如下配置，并保存文件为`~/.tmux.conf`：

```conf
# $Id: vim-keys.conf,v 1.2 2010-09-18 09:36:15 nicm Exp $
#
# vim-keys.conf, v1.2 2010/09/12
#
# By Daniel Thau.  Public domain.
#
# This configuration file binds many vi- and vim-like bindings to the
# appropriate tmux key bindings.  Note that for many key bindings there is no
# tmux analogue.  This is intended for tmux 1.3, which handles pane selection
# differently from the previous versions

# split windows like vim
# vim's definition of a horizontal/vertical split is reversed from tmux's
bind s split-window -v
bind v split-window -h

# move around panes with hjkl, as one would in vim after pressing ctrl-w
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# resize panes like vim
# feel free to change the "1" to however many lines you want to resize by, only
# one at a time can be slow
bind < resize-pane -L 10
bind > resize-pane -R 10
bind - resize-pane -D 10
bind + resize-pane -U 10

# bind : to command-prompt like vim
# this is the default in tmux already
bind : command-prompt

# vi-style controls for copy mode
setw -g mode-keys vi

# powerline style theme ❐
set -g status-left-length 52
set -g status-right-length 451
set -g status-fg white
set -g status-bg colour234
set -g window-status-activity-attr bold
set -g pane-border-fg colour245
set -g pane-active-border-fg colour39
set -g message-fg colour16
set -g message-bg colour221
set -g message-attr bold
set -g status-left '#[fg=colour235,bg=colour252,bold] #S #[fg=colour252,bg=colour238,nobold]#[fg=colour245,bg=colour238,bold] #(whoami) #[fg=colour238,bg=colour234,nobold]'
set -g window-status-format "#[fg=colour244,bg=colour233,nobold,noitalics,nounderscore] #I #[fg=colour240,bg=colour233,nobold,noitalics,nounderscore]  #[default]#W "
set -g window-status-current-format "#[fg=colour233,bg=colour63,nobold,noitalics,nounderscore] #[fg=colour117,bg=colour63,nobold,noitalics,nounderscore]#I  #[fg=colour231,bg=colour63,bold,noitalics,nounderscore]#W #[fg=colour63,bg=colour233,nobold,noitalics,nounderscore] "

# brew install reattach-to-user-namespace
# Setup 'v' to begin selection as in Vim
bind-key -t vi-copy v begin-selection
bind-key -t vi-copy y copy-pipe "reattach-to-user-namespace pbcopy"

# Update default binding of `Enter` to also use copy-pipe
unbind -t vi-copy Enter
bind-key -t vi-copy Enter copy-pipe "reattach-to-user-namespace pbcopy"
```

## tmux 2.4 版本之后

### macos high sierra

    bind-key -Tcopy-mode-vi 'v' send -X begin-selection
    bind-key -Tcopy-mode-vi 'y' send -X copy-pipe-and-cancel "reattach-to-user-namespace pbcopy"
    bind-key -Tcopy-mode-vi Escape send -X cancel
    bind-key -Tcopy-mode-vi V send -X rectangle-toggle

### ubuntu using xsel

    bind -T copy-mode-vi p send -X copy-pipe-and-cancel 'xsel -ip'
    bind -T copy-mode-vi s send -X copy-pipe-and-cancel 'xsel -is'
    bind -T copy-mode-vi o send -X copy-pipe-and-cancel 'xsel -ib'

### ubuntu using xclip

    bind -T copy-mode-vi y send -X copy-pipe-and-cancel 'xclip -in -selection clipboard'

