# 挂载iSCSI

## 所需软件包

```bash
$ yum install binutils iscsi-initiator-utils kmod-xfs xfsprogs -y
```



```bash 
$ systemctl start iscsi  # 启动iscsi服务
$ iscsiadm -m discovery -t sendtargets -p <-ip:port-> # 发现target
$ iscsiadm -m node -T <-iqn_target-> -p <-ip-> -l  	# 登录target
$ iscsiadm -m session 	# 查看当前连接
$ iscsiadm -m node -T <-iqn_target-> -p <-ip-> -u		# 退出target
$ iscsiadm -m node -T <-iqn_target-> -p <-ip-> --op update -n node.startup -v automatic # 设置开机自动登录iscsi
```

