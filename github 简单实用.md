老不太会，记一些简单用法

# 预先操作
----
- **本地代码**：对应一个git仓库，通常是需要使用`git init`命令创建，一般在项目文件夹下的 `.git` 
- **远程代码仓库**：在平台上创建一个repository，会有专属的url(这通常是一个以 `https://github.com/username/repository.git` 形式的链接)

将本地的仓库和远程仓库通过url进行绑定:`git remote add origin(别名，可改) [仓库URL]`

别名一般使用origin，但是也可以自己设定，之后每次提交就用设定好的。
# 更新操作
----
1. **添加项目文件到本地仓库**：
    - 在终端中运行 `git add .`，添加所有文件到本地仓库的暂存区。
    - 然后运行 `git commit -m "Initial commit"`，创建你的第一个提交，引号内的文字可以根据你的喜好更改，它是提交信息。
2. **推送代码到 GitHub 仓库**：
    - 运行 `git push -u origin master`（如果你的主分支名为 `master`）或者 `git push -u origin main`（如果你的主分支名为 `main`），将本地代码推送到 GitHub。
    - 首次推送时，可能需要你输入 GitHub 的用户名和密码。
3. **后续同步**：
    - 以后每次做了更改后，你只需要运行 `git add .`、`git commit -m "commit message"` 和 `git push` 来同步更改到 GitHub。

# 其他小笔记
---
## 别名
`git remote -v` 列出所有远程仓库的别名及其对应的 URL。
作用：确认已经正确设置远程仓库，或者需要检查已配置的远程仓库。
```shell
# 输出
origin  https://github.com/username/repository.git (fetch) 
origin  https://github.com/username/repository.git (push)
# 通常情况下，对于每个远程仓库，fetch 和 push 的 URL 是相同的。
```
“origin” 是远程仓库的别名，后面的 URL 是该别名指向的实际仓库地址。如果你配置了多个远程仓库，它们都会在这个列表中显示。
- `(fetch)` 表示用于获取（拉取）数据的 URL。
- `(push)` 表示用于上传（推送）数据的 URL。
### 别名的作用域是单个仓库
实际上，每个本地仓库可以独立地使用 `origin` 作为远程仓库的别名。`origin` 只是一个本地别名，用于指向远程仓库的地址，它在不同的本地仓库中是相互独立的。
简单来说，对于每个本地仓库：
- 可以在第一个本地仓库中设置 `origin` 来指向第一个远程仓库。
- 然后，在第二个本地仓库中，同样可以使用 `origin` 作为别名来指向另一个不同的远程仓库。

每个本地仓库的 `origin` 是独立的，它们不会相互冲突。这是因为每个本地仓库都有自己的 Git 配置和远程仓库信息。例如：
- 在本地仓库 A 中，`origin` 可以指向 `https://github.com/username/repositoryA.git`。
- 在本地仓库 B 中，`origin` 可以指向 `https://github.com/username/repositoryB.git`。

当你在仓库 A 中使用 `git push origin` 或 `git pull origin` 时，Git 会与仓库 A 的 `origin` 进行交互。同理，在仓库 B 中操作时，Git 会与仓库 B 的 `origin` 交互。这样，每个仓库都可以独立地使用 `origin` 来引用它们的远程仓库。

