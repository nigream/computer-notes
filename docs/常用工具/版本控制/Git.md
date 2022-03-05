

# Git

---

## 常用命令

```shell
# 初始化git
git init

# 设置远程分支
git remote add origin https://github.com/nigream/learning-notes.git

# 刷新分支
git remote update origin --prune

# 展示要删除的文件预览
git rm -r -n --cached "bin/"
git rm -r -n --cached "*.iml"

# 将文件移出版本控制
git rm -r --cached  "bin/"
git rm -r --cached  "*.iml"

# 展示仓库中的所有文件
git ls-files

# 删除分支（带警告）
git branch -d xxx
# 删除分支（不带警告，强行删除）
git branch -D xxx
```





## 常见问题



## .gitignore



