### 查看服务的日志
```shell
journalctl -u ss-rust
```
### 持续显示某个服务不断生成的日志
``` shell
journalctl -f -u ss-rust
```
