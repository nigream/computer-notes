# Conda命令

---

## pacakge相关

### search

```sh
# 搜索 包名
conda search pkgName
```

## env相关

### udpate

```sh
# 根据 environment.yml 下载相应版本的包
# --prune参数  表示如果文件中没有列出环境中已有的包，则将该包移除
conda env update --file environment.yml --prune
```

### info

```sh
# 列出当前conda创建的所有环境
conda info --envs
```

### activate

```sh
# 激活环境（即切换环境）
conda activate envName
```

