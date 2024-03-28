## 简介
很开心，今天终于又又一次上线了自己小Blog，为什么说“又”，因为曾经自己可是从头到尾用Django折腾过一个专属的，奈何因为服务器的羊毛到期了，只能被迫下线，唯有最近偶然机遇发现Github Pages和便捷的Gmeek，可以轻松部署。虽然模板没有像以前的那么花里胡哨，但胜在简单和便捷，可以让我更专注于搭建目的，而不是搭建过程，而不是本末倒置！（虽然我也折腾了一段时间，尝试考虑自定义🤣）

在折腾的过程中，又再次重新掌握github和git指令，这个过程让我对GitHub的用途之于工作用途有了新的思考和理解。

## Git指令
首先，先简单重温下git指令，主要是方便自己以后重新查看和掌握：
<img width="736" alt="git" src="https://github.com/ChanForWang/ChanForWang.github.io/assets/52204491/94a7bf9f-5313-4218-8474-2ae402baba31">

由图可见，总共有4个区域:
remote代表着远端github仓库;
repository是本地仓库；
workspace是正在工作区域；

一开始，透过 `git clone https://xxxxxxx.git` 将远端github仓库clone到本地；
或者在本地初建一个新的仓库 `git init`

随后，用`git add .` 将所有workspace的更改添加到index暂存区中，
随后透过 `git commit -m "<message you wanna add>"` 来去保存在本地仓库，
之后再通过 `git push origin main` 来去把所有更改推送到远端！

期间可以透过 `git status` 或 `git log` 来查看信息

若果远端发生更改，想同步到本地，则可以用`git fetch/pull`下来

## git分支管理
这是git很强大的一个功能，可以更好的进行代码的管理，有需要可以查阅资料，这里暂不赘述。

## git的tag用法
在弄本次博客过程中，第一次了解到git的tag标签用法，可以更好的进行代码版本管理，如v1.0，例如重大升级都可透过这个方式去管理自己的代码，形成快照！可回溯。

查看当前的所有tag： `git tag`；
创建新的tag： `git tag -a <tagname> -m "<any message>"` 
推送tag到远端时：`git push --tags` 

删除本地tag： `git tag -d <tagname>`
删除远端tag： `git push origin --delete <tagname>`

拉取远端指定tag: `git fetch origin tag <tagname>`
拉取远端所有tag：`git fetch --tags`

## 对git和github的理解
说到这点，其实之前在工作场景中，我一直苦恼于如何更好的备份和管理工作上的文件，特别是一些需要做好版本管理和可追朔的文件。然而，透过这个项目，让我重新认识了github，它的作用可不仅仅只局限于代码管理，我觉得它的可追朔功能，将会为我的文件管理带来巨大帮助！未来可以尝试一下！例如前端时间做的WMS！

## 结语
最后，很开心折腾的个人博客上线了，未来希望能利用这个平台，记录更多的所见所思。



