# pipx

![image-20240626193744472](assets/image-20240626193744472.png)

## 什么是pipx？

pipx 是一款用于帮助你安装和运行那些用 python 编写的终端程序，它类似于 macOS 上的 brew，Ubuntu 上的 apt，CentOS 上的 yum。

pipx 依赖 pip 和 venv，Pyhton 的版本需要 **3.6+**

## 安装

Linux

```bash
# deb
$ sudo apt update
$ sudo apt install pipx

# CentOS
sudo dnf install pipx
```

pip

```bash
$ python3 -m pip install --user pipx

$ pip install pipx
```

> 升级： python3 -m pip install --user --upgrade pipx

## pipx 选项

pipx 安装的应用程序的默认二进制位置是`~/.local/bin`。这可以用环境变量覆盖`PIPX_BIN_DIR`。







**文档**：[https://pipx.pypa.io](https://pipx.pypa.io/)

**源代码**：https://github.com/pypa/pipx

如何建立一个完美的 Python 项目：https://sourcery.ai/blog/python-best-practices/
