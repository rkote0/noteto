# Zsh 安装配置

![image-20240527144758709](assets/image-20240527144758709.png)

## 环境配置

```BASH
# 更新软件源
$ sudo apt update && sudo apt upgrade -y
# 安装 zsh git curl
$ sudo apt install zsh git curl -y

# 切换为 zsh
$ chsh -s /bin/zsh
```

> 设置默认终端为 zsh（**注意：不要使用 sudo**）
