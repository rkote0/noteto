

## Go 国内加速

国内的网络访问国外的资源经常会出现不稳定的情况。Go生态系统中有着许多著名的模块  `golang.org/x/...`。

所以 `CDN`加速代理就很有必要的。

- 七牛：Goproxy 中国 https://goproxy.cn
- 阿里： https://mirrors.aliyun.com/goproxy/
- 官方：  https://goproxy.io/
- 其他：jfrog 维护 https://gocenter.io

### 设置代理

**Unix**

在 Linux 或 macOS 上面,需要运行下面的命令(或者，把以下命令写到`.bashrc`或`.bash_profile`文件中)

```bash
# 启用 Go Modules 功能
go env -w GO111MODULE=on

# 配置 GOPROXY 环境变量，以下三选一

# 1. 七牛 CDN
go env -w  GOPROXY=https://goproxy.cn,direct
go env -w GOSUMDB=goproxy.cn/sumdb/sum.golang.org

# 2. 阿里云
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct
# GOSUMDB 不支持

# 3. 官方
go env -w  GOPROXY=https://goproxy.io,direct

```

> 校验：
>
> ```bash
> go env | grep GOPROXY
> GOPROXY="https://goproxy.cn"
> ```

**windows**

在 Windows 上，需要运行下面命令：

```bash
# 启用 Go Modules 功能
$env:GO111MODULE="on"

# 配置 GOPROXY 环境变量，以下三选一

# 1. 七牛 CDN
$env:GOPROXY="https://goproxy.cn,direct"

# 2. 阿里云
$env:GOPROXY="https://mirrors.aliyun.com/goproxy/,direct"

# 3. 官方
$env:GOPROXY="https://goproxy.io,direct"
```

> 校验：
>
> `time go get golang.org/x/tour`

**清除模块缓存**

```bash
go clean --modcache
```

**私有模块**

```bash
# 设置不走 proxy 的私有仓库，多个用逗号相隔
go env -w GOPRIVATE=*.corp.example.com
```

**取消代理**

```bash
go env -w GOPROXY=direct
```

**取消校验**

```bash
go env -w GOSUMDB=off
```

**设置不走 proxy 的私有仓库或组，多个用逗号相隔（可选）**

```bash
go env -w GOPRIVATE=git.mycompany.com,github.com/my/private
```



