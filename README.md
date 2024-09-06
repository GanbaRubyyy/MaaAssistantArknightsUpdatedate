```

```

**#** **MaaAssistantArknightsUpdatedate**

## How To Use? 将bat文件放入MAA项目根目录,需要Git软件进行更新

### 视频教程:【MAA停止资源自动更新了？简单小脚本解决 MAA资源自动更新脚本】 https://www.bilibili.com/video/BV18sHveFE3i

### 作者:https://space.bilibili.com/26557678

### 源代码:

```git
@echo off
setlocal
@chcp 65001 > nul

:: 初始化变量，默认不执行复制
set DoCopy=0

:: 检测当前路径下是否有 git_source 文件夹
if not exist git_source (
    echo "git_source 文件夹不存在，正在创建并初始化..."
    mkdir git_source
    cd git_source
    git init
    git remote add origin https://github.com/MaaAssistantArknights/MaaResource.git
    git pull origin main
    set DoCopy=1
) else (
    echo "git_source 文件夹已存在，正在检查更新..."
    cd git_source
    git fetch origin main
    git diff --quiet --exit-code master origin/main > nul 
    if errorlevel 1 (
        echo "有更新，正在拉取最新代码..."
        git merge FETCH_HEAD
        set DoCopy=1
    ) else (
        echo "已是最新版本，无需更新。"
    )
)
:: 返回上一级文件夹
cd ..

:: 根据变量DoCopy的值决定是否执行复制操作
if %DoCopy%==1 (
    echo "正在复制 cache 和 resource 文件夹到当前路径..."
    xcopy /E /I /Y .\git_source  . > nul
) else (
    echo "跳过复制操作。"
)

:: 运行当前目录下的MAA.exe
if exist MAA.exe (
    start MAA.exe
) else (
    echo "MAA.exe 不存在，无法运行。"
)

@REM timeout /t 100 > nul

endlocal
```

