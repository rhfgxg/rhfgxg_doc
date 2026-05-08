### error processing archive
报错详情：
```bash
报错详情
dpkg: error processing archive QQ.deb (--install):
 package architecture (arm64) does not match system (amd64)
Errors were encountered while processing:
 QQ.deb
```
原因：使用的 deb 版本错误：
例如：
X64结构的电脑 使用了 ARM64 的包
