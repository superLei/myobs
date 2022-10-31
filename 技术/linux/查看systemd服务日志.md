## 简介

> journalctl 用来查询 systemd-journald 服务收集到的日志。systemd-journald 服务是 systemd init 系统提供的收集系统日志的服务。在 Systemd 出现之前，Linux 系统及各应用的日志都是分别管理的，Systemd 开始统一管理了所有 Unit 的启动日志，这样带来的好处就是可以只用一个 journalctl 命令，查看所有内核和应用日志。

### 查看服务的日志
```shell
journalctl -u 服务名
```
### 持续显示某个服务不断生成的日志
``` shell
journalctl -f -u ss-rust
```
