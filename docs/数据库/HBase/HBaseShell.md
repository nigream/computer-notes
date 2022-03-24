# HBase Shell

---

参考：

1. hbase shell介绍：https://hbase.apache.org/2.0/book.html#shell 
2. hbase 过滤语句：https://hbase.apache.org/book.html#thrift.filter_language
3. hbase 命令大全：https://learnhbase.wordpress.com/2013/03/02/hbase-shell-commands/





## 进入Shell

```sh
# 进入 hbase 的 shell中
hbase shell
```

## 查询数据

```sh
scan 'xxxx',{FILTER => "(PrefixFilter ('40_cb105a77bc521ef15c1297_')"}
```

## Data Manipulation commands

### count

```sh
# INTERVAL 表示计数提示间隔，即每计到10条打印一次
# CACHE 表示缓存大小
count 't1', INTERVAL => 10, CACHE => 1000
```

## Fliter

```sh
scan 'xxxx',{FILTER => "(PrefixFilter ('abc_')"}
# ROWPREFIXFILTER 比 PrefixFilter 快多了，PrefixFilter会扫描全表
scan 'xxxx',{ROWPREFIXFILTER => "abc_"}
```

