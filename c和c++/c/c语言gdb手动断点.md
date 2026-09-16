# 编译可调试`exe`文件
```bash
gcc mian.c -g -o mian.exe
```
- 有要编译连接的要写进去
# 启动`gdb`,传入`exe`文件
```bash
gdb main.exe
```
# 在`gdb` 交互式操作

```gdb
# 在源文件第15行打断点
b main_tcp_client.c:15

# 运行程序，跑到断点停下
run

# 打印变量loop
p loop

# 下一行
n

# 进入函数
s

# 继续跑直到下一个断点
c

#退出gdb
q
```
