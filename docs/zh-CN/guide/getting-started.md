## 启动

NapCat 是基于 官方NTQQ 实现的Bot框架，因此先需要安装官方QQ，**注意同个账号不能同时登录NT QQ 和 NapCatQQ**

*如果没有安装 QQ 请往后翻查看安装方法*

修改 `config/onebot11.json`内容，并重名为 `onebot11_<你的QQ号>.json`，如`onebot11_1234567.json`

json 配置内容参数解释：

```json5
{
  // HTTP服务监听的 ip 地址，为空则监听所有地址
  "httpHost": "",
  // 是否启用http服务, true为启动，false为禁用，如果启用，可以通过http接口发送消息
  "enableHttp": false,
  // http服务端口
  "httpPort": 3000,
  // 正向 ws 服务监听的 ip 地址，为空则监听所有地址
  "wsHost": "",
  // 是否启用正向websocket服务
  "enableWs": false,
  // 正向websocket服务端口
  "wsPort": 3001,
  // 是否启用反向websocket服务
  "enableWsReverse": false,
  // 反向websocket对接的地址, 如["ws://127.0.0.1:8080/onebot/v11/ws"]
  "wsReverseUrls": [],
  // 是否启用http上报服务
  "enableHttpPost": false,
  // http上报地址, 如["http://127.0.0.1:8080/onebot/v11/http"]
  "httpPostUrls": [],
  // 是否启用http心跳
  "enableHttpHeart": false,
  // http上报密钥，可为空
  "httpSecret": "",
  // 消息上报格式，array为消息组，string为cq码字符串
  "messagePostFormat": "array",
  // 是否上报自己发送的消息
  "reportSelfMessage": false,
  // 是否开启调试模式，开启后上报消息会携带一个raw字段，为原始消息内容
  "debug": false,
  // 调用get_file接口时如果获取不到url则使用base64字段返回文件内容
  "enableLocalFile2Url": true,
  // ws心跳间隔，单位毫秒
  "heartInterval": 30000,
  // access_token，可以为空
  "token": ""
}

```

配置日志：

复制`config/napcat.json` 并重命名为 `config/napcat_<QQ号>.json`

json 配置内容参数解释：
```json5
{
  // 是否开启文件日志
  "fileLog": true,
  // 是否开启控制台日志
  "consoleLog": true,
  // 日志等级, 可选值: debug, info, error
  "fileLogLevel": "debug",
  "consoleLogLevel": "info"
}
```
### Windows 启动

运行`powershell ./napcat.ps1`, 或者 `napcat.bat`，如果出现乱码，可以尝试运行`napcat-utf8.ps1` 或 `napcat-utf8.bat`

*如果出现 powershell 运行不了，可以尝试 `powershell.exe -ExecutionPolicy Bypass -File ".\napcat.ps1"`*

**推荐使用 bat 运行，powershell 会自身占用 20MB 左右的内存**

### Linux 启动

运行`napcat.sh`

或使用[NapCatDocker](https://github.com/NapNeko/NapCat-Docker)

## 使用无需扫码快速登录

前提是你已经成功登录过QQ，可以加参数` -q <你的QQ>` 进行登录，如`napcat.sh -q 1234567`

## 安装

### Linux安装

#### 安装 Linux QQ(22741)，已经安装了的可以跳过

目前还在研究怎么精简安装，暂时只能安装官方QQ整体依赖

下载QQ
 
[deb x86版本](https://dldir1.qq.com/qqfile/qq/QQNT/Linux/QQ_3.2.7_240403_amd64_01.deb)
[deb arm版本](https://dldir1.qq.com/qqfile/qq/QQNT/Linux/QQ_3.2.7_240403_arm64_01.deb)

[rpm x86版本](https://dldir1.qq.com/qqfile/qq/QQNT/Linux/QQ_3.2.7_240403_x86_64_01.rpm)
[rpm arm版本](https://dldir1.qq.com/qqfile/qq/QQNT/Linux/QQ_3.2.7_240403_aarch64_01.rpm)

安装QQ
```bash
sudo dpkg -i --force-depends ./qq.deb
```

安装QQ的依赖
```bash
sudo apt install libgbm1 libasound2
```

### Windows 安装

#### 安装Windows QQ(22741)，已经安装了的可以跳过

[Windows版本QQ下载](https://dldir1.qq.com/qqfile/qq/QQNT/Windows/QQ_9.9.9_240403_x64_01.exe)

## 常见问题

### 二维码无法扫描

NapCat 会自动保存二维码到目录，可以手动打开图片扫描

如果没有条件访问本地目录，可以将二维码解析的 url 复制到二维码生成网站上生成二维码，然后手机QQ扫描

### 语音、视频发送失败

需要配置 ffmpeg，将 ffmpeg 目录加入环境变量，如果仍未生效，可以修改 napcat 启动脚本加入 FFMPEG_PATH 变量指定到 ffmpeg
程序的完整路径

如 Windows 上修改 napcat.ps1，在第一行加入

```powershell
$env:FFMPEG_PATH="d:\ffmpeg\bin\ffmpeg.exe"
```

### Windows系统出现QQ本体界面?
尝试管理员启动NapCat即可，如果失败请反馈NapCat开发群

### 修改日志等级



### 出现 error code v2:-1 之类的提示

不用管，这是正常现象，是因为 QQ 本身的问题，不影响使用

### 本地登录后迁移至服务器

如果在服务器扫码登录提示出现网络环境不稳定不在同一网络，可以尝试在本地登录后，将 QQ 的文档传到服务器相同目录覆盖，Linux 目录位于 `~/.config/QQ`, Windows 一般是 **文档下的QQ文件夹**，具体可以打开 `QQ的设置->存储管理` 查看

或者手机使用 VPN 等方式连接到服务器网络使其和服务器在同一网络

### Windows 运行出现 sqlite3 不是 win32 程序

运行时出现`node_sqlite3.node is not a valid Win32 application`

检查下载的是否是 Windows 版本的 NapCatQQ

检查是否是安装的 64 位版本的 QQ

### 如果出现崩溃

由于新版本使用了 Native Hook，如果你的 NapCatQQ 崩溃了，尝试删除 `MoeHoo.node`

### 其他问题

NapCat 是基于 QQ 22741 版本开发的，其他版本不敢保证是否会出现一些奇怪的问题，有问题可以尝试安装此版本的 QQ
