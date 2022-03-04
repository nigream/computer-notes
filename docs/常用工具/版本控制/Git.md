

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

### 合并分支时，如何忽略某文件？



### 新建一个开源项目的工作流

1. 在本地创建项目。

2. 在项目根目录下执行 `git init` ，初始化仓库。

3. 执行如下命令为项目单独配置 **用户名** 和 **邮箱** （在 `.git/config` 文件中可以查看项目的配置）。

   **PS：** 之所以要单独配置 **用户名** 和 **邮箱** ，是不想用 **全局设置** 的公司的  **用户名** 和 **邮箱** 。

   ```sh
   git config user.name "Nigream"
   git config user.email "nigreamlin@gmail.com"
   ```

   使用 `git config --list` 可以查看所有配置（使用 `Enter` 键可以下翻，使用 `q` 键可以退出）。

4. 添加 `.gitignore` 文件（忽略不应该提交的文件），模板如下：

   ```sh
   HELP.md
   target/
   !.mvn/wrapper/maven-wrapper.jar
   !**/src/main/**/target/
   !**/src/test/**/target/
   
   ### STS ###
   .apt_generated
   .classpath
   .factorypath
   .project
   .settings
   .springBeans
   .sts4-cache
   
   ### IntelliJ IDEA ###
   .idea
   *.iws
   *.iml
   *.ipr
   
   ### NetBeans ###
   /nbproject/private/
   /nbbuild/
   /dist/
   /nbdist/
   /.nb-gradle/
   build/
   !**/src/main/**/build/
   !**/src/test/**/build/
   
   ### VS Code ###
   .vscode/
   ```

5. 执行 `git add .` 将所有文件都加进版本控制中。

6. 执行 `git commit -a` ，输入必填的 commit 信息，将所有文件都提交至本地仓库。

7. 在远程仓库新建一个项目，执行 `git remote add origin-xxx http://xxx` ，为本地项目设置远程仓库。

8. 执行 `git push origin-xxx` 即可推送分支到远程仓库同名的分支上。

   - 为了指定本地 `master` 分支 `push` 时默认的 `远程仓库` ，可以执行 `git push -u origin-xxx master ` 。在配置文件中体现如下：

     ```sh
     [remote "origin-xxx"]
     	url = https://xxx
     	fetch = +refs/heads/*:refs/remotes/gitee/*
     [branch "master"]
     	remote = origin-xxx
     	merge = refs/heads/master
     ```

     这样就可以直接执行 `git push` 推送代码了。

9. 执行 `git branch dev_private` 在当前分支的基础上新建本地开发的分支（使用 `git branch` 可以查看当前本地的所有分支，使用 `git branch -a` 可以查看本地的所有分支和设置的远程分支）。

10. 执行 `checkout dev_private` 切换到新建的分支。

11. 执行 `git config merge.ours.driver true` 以设置合并分支时的驱动。

12. 新建 `.gitattributes` 文件，加入如下内容：

    ```sh
    # 因为本地开发时会有一些配置文件中会有私人的敏感信息，所以合并时不提交；
    **/resources/ merge=ours
    
    # 当需要提交代码到远程公开仓库时，先将这些配置文件复制出来，改掉敏感信息；
    # 然后切换到master分支，将dev_private分支合并到master分支（因为有以上操作，所以合并时只保留master分支上原本的配置文件，不会合并dev_private分支的配置文件）；
    # 将修改后的配置文件替换master分支上的配置文件，再提交代码到远程仓库即可。
    ```

    

13. 



## .gitignore



