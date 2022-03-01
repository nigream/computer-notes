# Linux基础

---

## 目录结构

### `/` 与 `~` 

`/` 表示Linux的根目录，根目录下一般有以下常用目录(可能不全) : `bin  boot  dev  etc  home  lib  opt  root  sbin  srv  sys  tmp  usr  var`

`~` 表示当前用户的 `家目录` ， `root` 用户的 `家目录` 是 `/root` ，普通用户 `a` 的 `家目录` 是 `/home/a` (每创建一个新的普通用户，都会在 `/home` 目录下创建一个以用户名命名的目录)



## 常用命令

### 系统相关

```shell
# 查看系统内核版本
[root@iZwz9havs2j3eja5yo3nrxZ ~]# uname -r
5.10.60-9.al8.x86_64
```

