## Git









Git 进行Clone 或者 Pull 出现:**SSL certificate problem: unable to get local issuer certificate**

```bash
# 关闭 Git 默认的 SSL 验证开关
git config --global http.sslVerify false
```