## 前言

因为我很喜欢在个人博客中实现音乐播放功能，而aplayer则是这其中一个超级轻便简洁的网页播放器，但有个问题就是：
>[!Warning]
>一旦页面刷新或页面跳转新链接，整个音乐的播放状态会被重置

因此，我一直在寻找一个方式如何能解决此问题，从中我了解到了Pjax。
但Pjax我在以前第一次建立博客的时候就曾经试用过，会衍生出各种奇奇怪怪的bug，因此果断放弃，寻找其他方式；果然皇天不负有心人，终于找到了以下解决方案！

>[!Important]
>思路：
>1. 透过获取APlayer的实例对象,了解目前的一些播放状态，如时间，曲目等
>2. 从Localstorage和Sessionstorage来去获取播放的状态，从而实现不间断播放

整体代码如下：
```
<script>
    var ap = new APlayer({
        element: document.getElementById('aplayer'),
        showlrc: false,
        fixed: true,
        mini: true,
        audio: [
            {
                title: '向云端',
                author: '小霞/海洋Bo',
                url: 'https://dlink.host/sharepoint/aHR0cHM6Ly9waWNvZmFyZWFzdC1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC9qYW1lc19jaGFuX3BpY29fY29tL0VXOGRLM0xkM2RwQnBHUDdDR2l5Si1jQkF3d0xoZGZlSHE5Qk9VeHJnVVN3N3c_ZT12d05zMXo.mp3',
                pic: 'https://p2.music.126.net/TmOHxaGnFNlwNX8aPz66oA==/109951168638913915.jpg?param=130y130'
            },
            {
                title: "南山南",
                author: '张磊',
                url: 'https://dlink.host/sharepoint/aHR0cHM6Ly9waWNvZmFyZWFzdC1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC9qYW1lc19jaGFuX3BpY29fY29tL0VhMGlpeE5zR3NkSGdQYXhqZ0JsZ1BFQnYwZW9KaUlOTHNTcHRBZjhrMWtxSVE_ZT1kb08wUFQ.mp3',
                pic: 'https://p1.music.126.net/sF17Bq1KRm9vdBMjvhXTvQ==/109951165134399856.jpg?param=130y130'
            },
            {
                title: "稻香",
                author: "周杰伦",
                url: "https://dlink.host/sharepoint/aHR0cHM6Ly9waWNvZmFyZWFzdC1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC9qYW1lc19jaGFuX3BpY29fY29tL0VhYlQzeGdOeXpwTnZpMFBWZ21sQ3VjQnBZR0VEWnplNUppZ0s5VWZUOXlpZFE_ZT1tTno1a2w.jpg",
                pic: "https://p2.music.126.net/DpAuXTXlirLKXawYxJo70g==/109951164579389026.jpg?param=130y130"
            },
            {
                title: "晴天",
                author: "周杰伦",
                url: "https://dlink.host/sharepoint/aHR0cHM6Ly9waWNvZmFyZWFzdC1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC9qYW1lc19jaGFuX3BpY29fY29tL0VZR0FXSU45X2JKTWdlRGdjMnhNbHRvQk40VmNjc0xfSnljS1cwbXJWbHJDRWc_ZT14QnQ3dGI.jpg",
                pic: "https://p1.music.126.net/DpAuXTXlirLKXawYxJo70g==/109951164579389026.jpg?param=130y130"
            },
            {
                title: "向阳而生(日出)",
                author: "华晨宇",
                url: "https://dlink.host/sharepoint/aHR0cHM6Ly9waWNvZmFyZWFzdC1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC9qYW1lc19jaGFuX3BpY29fY29tL0VRVjctd19ZT2Q1T21Gd2VYSHdxU1dnQkplOVNTc0tfTTNucnV2c0Y3bFFETUE_ZT1hZ0QxVWs.mp3",
                pic: "https://p1.music.126.net/w1A7-Eqx26C0-ANXd3NolA==/109951169646542385.jpg?param=130y130"
            },
            {
                title: "若把你",
                author: "刘瑾睿",
                url: "https://dlink.host/sharepoint/aHR0cHM6Ly9waWNvZmFyZWFzdC1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC9qYW1lc19jaGFuX3BpY29fY29tL0VlZXlvbkpZNllkR3VMNDZDMk00VjFZQkRZYVRkTU5kYkxBanBrZHFSSFlLTlE_ZT11bnVaN2o.mp3",
                pic: "https://p2.music.126.net/M877M2-VhWZiLPVFORf9iQ==/109951163401482434.jpg?param=130y130"
            },
        ]
    });


    function doStuff() {
    var flag = 0; // 用于标记是否成功获取到了aplayer对象

    try {
        ap = ap; //获取aplayer实例对象
        ap.list; //获取aplayer对象的list属性，用于控制播放列表
        flag = 1; // 如果没有抛出异常，则设置flag为1表示成功获取到了aplayer对象
        console.log('成功获取aplayer实例对象', ap.list)
    } catch (error) {
        setTimeout(doStuff, 50); // 如果获取aplayer对象失败，则等待50毫秒后再尝试
        console.error("不获取aplayer实例对象，并捕获到错误：", error);
        return;
    }

    if (flag) { // 如果成功获取到了aplayer对象
        document.getElementsByClassName("aplayer-icon-menu")[0].click() // 模拟点击播放器右上角的菜单按钮

        // 检查是否有保存的音乐索引
        if (localStorage.getItem("musicIndex") != null) {
            musicIndex = localStorage.getItem("musicIndex"); // 从localStorage中获取音乐索引，即目前在播放哪一首歌！
            ap.list.switch(musicIndex); // 切换到指定的音乐索引
        }

        // 检查是否有保存的音乐时间
        if (sessionStorage.getItem("musicTime") != null) {
            window.musict = sessionStorage.getItem("musicTime"); // 从sessionStorage中获取音乐时间
            ap.setMode(sessionStorage.getItem("musicMode")); // 设置播放模式
            if (sessionStorage.getItem("musicPaused") != '1') { // 检查是否是暂停状态，1是播放，0是暂停
                ap.play();
            }

            var g = true; //加个变量以防鬼畜但是不知道怎么节流qwq；// 添加一个变量g，用于控制是否可以进行seek操作
            ap.on("canplay", function() { // 当音乐可以播放时
                if (g) {
                    ap.seek(window.musict); // 跳转到指定的音乐时间
                    g = false; //如果不加oncanplay的话会seek失败就这原因炸很久；// 修改g的值，避免多次调用seek方法
                }
            });
        } else {
            sessionStorage.setItem("musicPaused", 1); // 如果没有保存音乐时间，则将音乐设为暂停状态
            ap.setMode("mini"); // 将播放器设置为迷你模式
        }

        // 检查是否有保存的音量设置
        if (sessionStorage.getItem("musicVolume") != null) {
            ap.audio.volume = Number(sessionStorage.getItem("musicVolume")); // 设置音量
        }

        // 当音乐暂停时
        ap.on("pause", function() {
            sessionStorage.setItem("musicPaused", 1); // 更新sessionStorage中的音乐为暂停状态
        }); //原基础上加了个检测暂停免得切换页面后爆零(bushi)（指社死）

        // 当音乐播放时
        ap.on("play", function() {
            sessionStorage.setItem("musicPaused", 0); // 更新sessionStorage中的音乐为播放状态
        });

        // 当音量发生变化时
        ap.audio.onvolumechange = function() {
            sessionStorage.setItem("musicVolume", ap.audio.volume); // 更新sessionStorage中的音量设置
        }; //新版增加保存音量免得切换页面爆零


        // 使用setInterval函数定期执行以下代码，即每隔200ms就更新一次播放状态和进度
        setInterval(function() {
            musicIndex = ap.list.index; // 获取当前播放的音乐索引(歌曲的索引，index=0 为第一首歌)
            musicTime = ap.audio.currentTime; // 获取当前音乐的播放时间
            musicMode = ap.mode; //获取当前音乐的播放模式
            localStorage.setItem("musicIndex", musicIndex); // 保存当前播放的音乐索引到localStorage
            sessionStorage.setItem("musicTime", musicTime); // 保存当前音乐的播放时间(进度)到sessionStorage
            sessionStorage.setItem("musicMode", musicMode); // 保存当前的播放模式到sessionStorage
        }, 200); // 每200毫秒更新一次播放状态和进度；节流，200ms精度感知不大
    };
};

doStuff();

</script>
```

## Remarks

小小的美中不足便是跳转或刷新页面时，会暂停1秒作用，这是因为在读取状态。
未来看如何优化之

## 参考

[不开pjax的APlayer不中断解决方案](https://yisous.xyz/posts/614f1131/)