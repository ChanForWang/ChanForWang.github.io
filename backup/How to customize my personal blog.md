## Repository: ChanForWang.github.io

`config.json: update the configuration of the blog， i.e: Name of blog.`

`>github/workflow>Gmeek.yml（Action自动化执行的代码）>Name: Clone source code: 将链接改为自己的github：https://github.com/ChanForWang/Gmeek.git /opt/Gmeek`

## Repository: Gmeek

1.将此仓库git clone到本地
2.templates：爆改的一切在这
3.geek.py 是整个逻辑部分

## 执行顺序
当一切本地修改好的内容push到远程github 仓库后
（这里的push指的是代码部分的push和tag的push，只有tag的push完成后，才会在后续生效，因为Gmeek.yml里面的clone source code逻辑部分，是依照tag的最新版本来生成的！）

随后在repository：chanforwang.github.io下的Action，去build Gmeek即可！


## Remarks
发现一个很不错的音乐外链方式：Onedrive
1.将音乐MP3下载到本地
2.右击音乐，选中onedrive的share
3.copy link到https://onedrive.gimhoy.com/ ； 将share link转换为mp3 link
4.此link便是Gmeek仓库下>template>music.html里的音乐外链！
5.图片外链，直接用网易云音乐的缩图便可！