# 一、断点方式
## 1.breakpoint() (python3.7+)

  - 在代码中添加breakpoint()方法实现断点
```python
...
breakpoint()
...
```
## 2.pdb库

 - 引用pdb库中的方法实现断点
```python
import pdb
...
pdb.set_trace()
...
```

# 二、常用基础指令
- 运行程序停在断点后，底部出现  (Pdb)  交互提示符，输入以下指令：
## 1.查看当前断点附近代码
```pdb
list
```
or
```pdb
l
```
## 2.单步跳过（执行下一行，不进入函数内部）
```pdb
next
```
or
```pdb
n
```
## 3.单步进入（下一行如果是函数，跳进函数内部）
```pdb
step
```
or
```pdb
s
```
## 4.打印变量值，例  p 变量
```pdb
print a
```
or
```pdb
p a
```
## 5.继续运行，直到下一个断点
```pdb
continue
```
or
```pdb
c
```
## 6.退出调试、终止程序
```pdb
quit
```
or
```pdb
q
```
## 7.直接运行到当前函数末尾返回
```pdb
return
```
or
```pdb
r
```
## 8.清除所有断点
```pdb
clear
```
## 9.设置新断点，例  b 行号
```pdb
b 20
```