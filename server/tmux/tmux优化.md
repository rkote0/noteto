# Tmux 优化

## 插件管理器

安装 `tmux` 插件管理器

```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

配置文件参考：

> 安装插件：快捷键 -> 进入 tmux
> `control + B + shift + i`

```.tmux.conf
#  ~/.tmux.conf
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
# set -g @plugin 'christoomey/vim-tmux-navigator'
# set -g @plugin 'tmux-plugins/tmux-yank'
# set -g @plugin 'jimeh/tmuxifier'

# Other examples:
set -g @plugin 'catppuccin/tmux'
set -g @catppuccin_flavour 'mocha'

run '~/.tmux/plugins/tpm/tpm'

# non-plugin options
set -g default-terminal 'tmux-256color'
set -g base-index 1
set -g pane-base-index 1 # 修改终端的起始编号从 1 开始
set -g renumber-windows on # 使tmux关闭窗口后的编号连续
set -g mouse on

# Visual mode
set-window-option -g mode-keys vi
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi C-v send-keys -X rectangle-toggle
bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel

# keymaps
# unbind C-b	# 取消control + B 的前置
# set -g prefix C-Space # 绑定control + 空格
```

## tmuxifier

```
git clone https://github.com/jimeh/tmuxifier.git ~/.tmuxifier
echo 'export PATH="$HOME/.tmuxifier/bin:$PATH"' >> ~/.zshrc


# 安装 tmuxifier 插件之后
tmuxifiter new-session devops
tmuxifier edit-session devops
```

```bash
session_root ''" # session 的工作目录
if initialize_session "devops"; then
	new_window "dev"
	new_window "htop"
	run_cmd "htop"
	select_window "dev"
	run_cmd "ls"
fi
finalize_and_go_to_session
```



插件推荐：

```set -g @plugin 'christoomey/vim-tmux-navigator'
set -g @plugin 'christoomey/vim-tmux-navigator'
set -g @plugin 'tmux-plugins/tmux-yank'
set -g @plugin 'jimeh/tmuxifier' # tmux session 管理工具
```

项目地址：

- 插件管理工具：https://github.com/tmux-plugins/tpm
- tmux session管理工具：https://github.com/jimeh/tmuxifier