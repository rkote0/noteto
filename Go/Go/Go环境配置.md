# Go 环境配置

官网： https://go.dev/dl/

中文网： https://go.p2hp.com/go.dev/dl/

包管理器：**https://github.com/voidint/g**

Govm: https://github.com/govm-project/govm



## 包管理器

**Linux/macOS (bash/zsh)**

```bash
curl -sSL https://raw.githubusercontent.com/voidint/g/master/install.sh | bash

# git 会冲突，所以需要取消绑定别名
echo "unalias g" >> ~/.bashrc

source "$HOME/.g/env"
```

**Windows (pwsh)**

```pwsh
iwr https://raw.githubusercontent.com/voidint/g/master/install.ps1 -useb | iex
```





## 命令



```bash
查询可用的稳定版 `Go` 进行安装
$ g ls-remote stable

安装指定版本
$ g install 1.22.2

查看已安装的 `Go` 版本列表
$ g ls-remote

切换已安装的版本
$ g use 1.22.2

卸载特定的版本
$ g uninstall 1.22.2

清除 Go 安装包文件缓存
$ g clean

包管理器版本信息
$ g version

更新包管理器
$ g self update

卸载包管理器
$ g self uninstall
```

