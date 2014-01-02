通过启动sublime text 3时指定LD_PRELOAD来修复ubuntu 13.10中sublime text 3不支持fcitx中文输入法的问题。同时这里通过U官方的deb安装。安装地址为/opt/sublime_text

### 1.安装C/C++的编译环境和gtk libgtk2.0-dev

    sudo apt-get install build-essential
    sudo apt-get install libgtk2.0-dev

### 2.编译共享内库

    gcc -shared -o libsublime-imfix.so sublime-imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC

### 3.将libsublime-imfix.so移动到/usr/lib/

    sudo mv libsublime-imfix.so /usr/lib/

### 4.启动 Sublime Text 3

    LD_PRELOAD=/usr/lib/libsublime-imfix.so subl

但是这样的话，我们每次都要在终端里面使用命令启动sublime text 3，这样很不方便，接下来我们还要通过修改subl命令达到使用subl等命令启动以及修改sublime-text.desktop达到点击图标启动。

### 1.打开sublime-text-2

    sudo vim /usr/bin/subl

### 2.修改subl为以下内容

    #!/bin/bash

    LD_PRELOAD=/usr/lib/libsublime-imfix.so /opt/sublime_text/sublime_text "$@"

这样就能使用subl等命令启动

### 3.打开sublime-text.desktop

    sudo vim /usr/share/applications/sublime-text.desktop

### 4.修改sublime-text.desktop

将

    /opt/sublime_text/sublime_text

替换为

    subl

好了，接下来你在dash中点击打开sublime text吧，开始你的代码之旅吧。

代码fork:https://github.com/pavelhurt/sublime2-fcitx-fix
