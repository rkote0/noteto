# pyenv

pyenv 是 forcked 自 Ruby 社区的简单、易用、遵循 Unix 哲学的Python 环境管理工具,它可以轻松切换全局解释器版本, 同时结合vitualenv插件可以方便的管理对应的包源

## 安装

```bash
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv

# bash 执行
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# zsh 执行
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```



- [pyenv](https://github.com/pyenv/pyenv)

- [pyenv install ](https://github.com/pyenv/pyenv-installer) 

- 命令参考： https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-latest
