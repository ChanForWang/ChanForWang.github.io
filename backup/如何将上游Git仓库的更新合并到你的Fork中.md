## 引言

在开源世界中，Git的fork功能是一个强大的工具，它允许开发者复制一个仓库到自己的账户下，进行修改和实验，而不会影响原始仓库。然而，随着时间的推移，

>[!Important]
>痛点：**原始仓库（上游仓库）会有新的更新，而你的fork（下游仓库）也可能有独特的更改。**
>需求：本文将指导你如何 **将上游仓库的更新同步到你的fork中，同时保留你自己本地fork仓库的更新。**

>[!Note]
>实际case：上游的Meekdai/Gmeek仓库会有不定期的更新，但我自己fork到github的ChanForWang/Gmeek则不会同步更新，且有自己的独特更新，因此若想使用最新上游所更新的功能，则需要本次文章介绍的方式！

## 同步上游更新的步骤

### 1. 克隆你的Fork到本地

首先，如果你还没有克隆你的fork，可以通过以下命令来克隆：

```sh
git clone https://github.com/ChanForWang/Gmeek.git
```

### 2. 添加上游仓库作为远程仓库

进入仓库目录，并添加上游仓库的远程引用：

```sh
cd Gmeek
git remote add upstream https://github.com/Meekdai/Gmeek.git
```

### 3. 获取上游仓库的最新提交

通过以下命令获取上游仓库的最新状态：

```sh
git fetch upstream
```

### 4. 创建一个新的分支用于同步-重要step！

为了避免直接在主分支上操作，创建一个新的分支来尝试合并上游的更改：

```sh
git checkout -b sync-upstream
```

### 5. 合并上游仓库的更改

将上游仓库的`main`分支合并到你的新分支：

```sh
git merge upstream/main
```

### 6. 解决合并冲突（如果有）

如果合并过程中出现冲突，Git会提示你解决这些冲突。

- 编辑冲突文件；（Vim xxx文档）
- 删除Git的冲突标记；
  - <<<<Head  ======,这部分是我本地有冲突的代码，======= >>>>>>upstream, 这部分则是上游的代码
  - 看要选取那一部分代码作为替换的，随后删掉这些冲突标记<<<<<====>>>>>
- 并保存更改

### 7. 推送更改到你的Fork

将解决冲突后的更改推送到你的GitHub上的fork：

```sh
git push origin sync-upstream
```

### 8. 在GitHub上发起Pull Request

转到你的fork的GitHub页面，比较并创建一个Pull Request，将`sync-upstream`分支的更改合并到你的`main`分支。

### 9. 更新你的`main`分支

一旦Pull Request被合并，你可以更新你的`main`分支以包含最新的更改：

```sh
git checkout main
git merge sync-upstream
git push origin main
```

### 10. 创建新的版本标签

现在你的`main`分支已经包含了所有最新的更改，你可以创建一个新的版本标签来标记这个发布版本：

```sh
git tag -a v1.0.0 -m "版本1.0.0发布"
git push origin v1.0.0
```

## 结语

通过上述步骤，你可以有效地将上游仓库的更新同步到你的fork中，同时保留你自己的更改。这不仅有助于保持你的fork是最新的，而且也使得贡献回上游仓库变得更加容易。记住，良好的沟通和清晰的文档是开源协作的关键。

---

这篇文章可以作为博客上的一篇科普文章，帮助读者理解如何在Git中管理fork和上游仓库之间的同步。

---
### 额外补充：以Vim命令编辑文件的用法

按下 Esc 键：首先，确保你处于普通模式。
如果你在插入模式（Insert mode）、替换模式（Replace mode）或其他模式下，按下 Esc 键会将你带回普通模式。

输入退出命令：在普通模式下，你可以输入以下命令之一来退出 vim：

:q — 退出 vim（如果对文件做了更改且没有保存，这将不保存更改直接退出）。
:q! — 强制退出 vim，不保存任何更改。
:wq — 保存更改并退出 vim（写入并退出）。
:wq! — 强制保存更改并退出 vim，即使文件是只读的。