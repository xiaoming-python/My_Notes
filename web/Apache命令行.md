

```cmd
httpd -t              #校验配置文件语法（最常用！）
httpd -k start        #启动apache服务
httpd -k stop         #停止
httpd -k restart      #重启
httpd -k install      #安装成windows系统服务
httpd -k uninstall    #卸载系统服务
```
# indesx默认文件目录
- `...\Apache24\htdocs`
# 设置访问 `http://localhost` 默认打开 index.php

原理：`DirectoryIndex` 控制默认首页，**写在最前面优先级最高**。

1. 打开 `E:\web\apache\Apache24\conf\httpd.conf`
2. 搜索找到这一块 `<IfModule dir_module>` 片段：

apache

```apache
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
```

改成：

```apache
<IfModule dir_module>
    DirectoryIndex index.php index.html index.htm
</IfModule>
```

> ✅顺序很重要：先找 `index.php`，找不到再去找 `index.html`。

> 注意：你之前在文件末尾 PHP 配置那里也写过一行`DirectoryIndex`，**删掉重复的那一行**，只保留`<IfModule dir_module>`里面这一处，不要写两份，避免冲突。

3. 保存配置文件
4. 管理员 PowerShell 校验配置：

powershell

```powershell
.\httpd -t
```

看到 `Syntax OK`，重启 Apache

```powershell
.\httpd -k restart
```
