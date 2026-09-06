# 查看IP
## `ipconfig`
or
## `curl ipinfo.io/ip`

# 查询DNS信息
## nslookup
查询网址的DNS信息
实例：
```cmd
nslookup www.baidu.com
```
# 测试网络连通性
## ping
测试到目标网址的连通性
实例：
```cmd
ping 127.0.0.1
```
# 查看本机网络连接进程
## netstat
- `-a`显示所有连接的监听端口
- `-n`已数字形式显示地址和端口号，速度更快
- `-o`显示每个连接对应的进程ID(PID)
实例：
```cmd
netstat -ano
```
# 追踪数据包到目标主机经过的路径
## tracert
实例：
```cmd
tracert 127.0.0.1
```
# 清理pip缓存
## `pip cache purge`
# 终端输出编码格式
## `chcp`查看输出编码格式
## `chcp 65001`改变输出编码为UTF-8格式

# 进程
## `tasklist`列出所有进程
## `tasklist I findstr "PID"`查询对应PID进程
实例：
```cmd
tasklist | findstr "1234"
```
