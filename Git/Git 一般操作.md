# Git 一般操作

Git 是常用的版本管理工具，也各大公司常用的版本管理神器，与之相关的还有svn。对于一般的开发者，涉及的操作主要操作并不多，对于git有基本的理解和操作即可。这里罗列了一些我在开发中使用的一些常见操作，也许并不适用于所有人。

首先，我们连接远程的仓库，需要在远程的仓上需要添加自己本地的ssh密钥。在linux上直接执行

```jsx
sshkeygen
```

然后一路回车，即可生成相应的密钥。然后在用户根目录下面.ssh文件夹中（默认路径）生成了对应的ssh 公钥，然后将公钥拷贝到远程项目的网页上即可，这样就获得了访问该项目的权限。

1. 拉取远程仓库。
    
    ```jsx
    git clone https://....git 
    ```
    
2. 建立新分支-一般情况我们并不在拉取下来的主干上直接进行开发，开发的时候我们也需要尽可能保持主干上的干净。
    
    ```jsx
    git checkout -b br-dev.
    ```
    
3. 后面的命令就不一直讲述了：

    ```jsx
    git add -u
    git commit -m “comment”  // 注释需要充分，意思要表达清楚，与代码表达的意思对应。
    git commit —amend ;      // 不需要新提commit，与上一个commit 合并到一起。
    git push;      // push到远程。
    git push -f;   //强制push到远程，这个命令也经常用，有时候git push 的时候，如果远程分支已经存在，提交可能会有冲突。这时候你可能无法提交上去。用 -f 强制推，覆盖原来的远程分支。
    ```

1. 删除已经add 进来的文件。 

    ```jsx
    git rm -f 
    git rm -d
    ```

1. 拉去别人的仓    
更加标准的操作可能是fork 主干仓，然后在自己的仓上开发，这样能保证主仓是干净的。这一点我也深有体会，如果所有人的分支都提到主仓上，这样导致上面的分支非常多，根本分不清谁是谁的分支，很多分支都没有存在的必要，成百上千的分支在一起，这样看起来给人的感觉真是非常糟糕。但是我们有时候需要参考一下别人的代码。差别别人仓库上的代码修改。这就需要在本地添加一个远程仓库的地址。
  
    ```jsx
    git remote add xxx https://xxx.git
    git fetch xxx
    git checkout br-xxx
    ```    
    这样我们就能切到别人的分支上了。有时候我们不仅仅是要看别人的代码，我们还依赖他们的代码，这样就要把别人的提交，pick到我们的分支。

    ```jsx
    git log 记住我们要pick 的结点。
    ```

    然后我们直接

    ```jsx
    git cherry-pick xxx . 这样我们就获得了别人的提交。
    ```

1. 修改分支，修改结点信息，合并commit， 删除commit，这里就使用 

   ```jsx
    git reset -i xxx  
   ```

    或者 

    ```jsx
    git reset —soft HEAD^3
    ```

    进入修改界面之后，会有各种提示，根据你自己的需要来确定是合并commit，还是删除commit，还是修改commit 信息。

1. 回滚结点——防止影响其它开发者

    ```jsx
    git revert -n xxx 
    ```

    如果之前的结点有错， 我们需要回滚这个结点，这样做的目的是因为我们不想影响其它开发的人，然后选择回滚，而不是直接删除。

1. git 操作记录
    
    ```jsx
    git reflog
    ```
    
2. 打patch，应用patch
    ```jsx
    git show xxx > patch.diff 2>&1
    
    git appy patch.dff
    ```
    
3. git 配置
    
    ```jsx
    // git 代理设置
    git config --list
    git config --global https.proxy https:// ..
    git config --global --unset https.proxy
    git config --global http.sslVerify false
    ```