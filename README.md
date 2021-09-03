FolderMagic 1.3
----------

简单易用，无需部署的列表程序。
特性：
* 无需环境，无需数据库，低内存占用
* 支持webdav管理
* 支持完善的文件管理，可新建删除重命名和移动任意文件或文件夹，支持批量操作（移动端）
* 支持视频在线播放，支持字幕（srt, ass, ssa, vtt等），支持倍速播放
* 支持图片预览，支持常见jpg, gif, png, tif, psd格式预览，图片画廊带来流畅体验
* 支持音频在线播放，支持解析专辑图片和歌手信息，目前支持mp3,wav和ogg格式
* 支持文档在线预览，包括常见各类代码格式，如html, js, css, php, py, pdf等
* 支持office在线预览
* 支持共享链接，支持共享链接管理
* 集成aria2ng，并安全的指向内部转发地址
* 文件搜索，即时搜索整个列表文件夹
* 中英多语言支持
* 支持常用文件管理，文件一拖即传
* 支持主流浏览器，完整支持IE11，部分支持IE10和IE9
* 响应式布局，适配移动浏览器，适配黑暗模式

**这不是一个Onedrive/GoogleDrive/Dropbox/世纪互联/OSS 的列表程序，这是硬盘目录列表程序**

## 使用方式

本程序为linux amd64可执行文件，[点击这里](https://github.com/FolderMagic/FolderMagic/raw/master/FolderMagic) 下载后执行
`chmod +x FolderMagic`

然后就可以 `./FolderMagic` 运行了，默认共享当前所在文件夹，公开访问无认证。所有可选参数如下：

### 命令行参数
```
  -b, --bind=     监听端口，格式为 IP:PORT 或 :PORT，请注意冒号不能省略 (default: :80)
  
  -r, --root=     列表根目录，默认为当前目录
  
      --gzip=     启用gzip压缩 (default: true)
	  
  -a, --auth=     认证信息，格式： "用户名:密码" 认证信息用于网页登录和webdav，不设置则无认证，webdav将被禁用
  
  -w, --webdav=   webdav认证路径，需在webdav客户端中输入 (default: /manager)
  
      --page404=  自定义404页面
	  
      --nosearch  关闭内置搜索功能
	  
      --nothumb   关闭内置缩略图生成功能，使用简易画廊，见下文描述 (默认 false)
	  
  -s, --share=    默认共享链接有效期，单位分钟 (default: 60)
  
      --aria=     Aria2 RPC地址 (默认 "http://127.0.0.1:6800/jsonrpc")，列表程序将安全的转发这个地址
	  
  -d, --daemon    以后台服务方式运行，无需nohup或screen。不支持Windows。
  
      --pid=      后台运行时的pid文件，不支持Windows (default: /var/run/fm.pid)
```
说明：使用Windows时，参数需使用空格隔开，如 -r c:\temp，使用Linux时，参数可使用空格或=隔开。-d，--nothumb等指令不需要参数可直接使用。

## 缩略图

### 登录界面
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/login.png)

### 文件浏览
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/browse.png)

### 文件移动
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/filemove.png)

### 画廊
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/gallery.png)

### 简易画廊
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/simplegallery.png)

### 字幕支持
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/subtitle.png)

### 文件搜索
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/search.png)

### 移动端优化
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/mobile1.png)

### 共享管理
![image](https://github.com/FolderMagic/FolderMagic/blob/master/thumbnails/shareman.png)

## webdav 使用

使用命令行 `--auth user:password` 启用鉴权后webdav即自动启用。

使用raidrive或其他webdav兼容客户端连接 http://your.domain:port/manager 输入用户名和密码即可连接。

由于webdav本身协议的限制，webdav下不能对文件名为乱码的文件和文件夹进行操作，请在网页端进行重命名。

/manager 可使用 `-w`或`--webdav` 指令更改

## 文件管理

在网页列表界面右键即可操作文件和文件夹，可以新建和删除。

支持新建文件夹、删除文件和文件夹、重命名及移动文件。**网页端支持非标准文件名操作（如乱码的文件名）**

不使用认证时只能下载文件，不能进行其他操作

## 文件上传

1. 使用webdav客户端上传。

2. 直接把文件拖到列表界面，出现蓝框提示松手上传即可。

上传过程中可继续拖入新的文件排队上传。不支持拖文件夹上传。

IE9 及以下浏览器由于浏览器限制无法上传。

## 视频预览

支持预览mp4, mkv格式，能否成功播放取决于实际视频容器内的编码格式。

字幕需要在视频同一文件夹下，字幕文件名包含视频名称即可，如`a.mp4`和`a_en.srt`即为匹配字幕。多个字幕将被同时载入可以在播放界面选择。字幕支持所有常见字幕，ass特效字幕保留文字部分，特效无法支持。

## 音频预览

支持mp3和ogg，具体支持视浏览器而定。点击即可在左下角开始播放。

音频将被自动解析专辑图片和歌曲名称，歌手。（IE9不支持解析）

点击新的音频可以自动加入播放列表，切换文件夹不影响播放。 （IE9 将中断播放）

## 图片预览

支持各种常见图片预览，图片将自动生成合适缩略图并使用webp格式（如果浏览器支持）传输

支持psd格式预览，gif格式生成缩略图后没有动画。

默认使用服务端生成图片缩略图，浏览大图也同样流畅，节省流量。若服务端性能较弱或内存不足，可以使用-nothumb指令关闭缩略图功能。前端将自动使用简化版画廊。
简化版画廊功能较少，并且客户端直接下载原图浏览。简化版画廊不能预览psd格式。

## Office预览

基于微软在线预览实现。按照微软的预览要求，需要将拥有域名并且FolderMagic需要在80或443端口，否则无法预览。

即：浏览器中显示的地址必须为 http://example.com/example.doc 或者 https://example.com/example.ppt 这样的形式才能被预览，否则会显示无法打开文件。

## 共享管理

通过右键复制的临时链接自动拥有一定时间的有效期（默认60分钟，可通过`--share`指令更改），到期后无法被下载。

在右下角菜单中选择共享管理即可添加或减少共享时间，也可删除共享

复制的永久链接除非移动文件或更改用户名密码，否则永远有效，不可删除。

重启FolderMagic后，所有临时共享都会失效，永久连接依然有效

## AriaNG

通过右下角菜单可以调用内置的ariaNg，并默认指向/jsonrpc路径。FolderMagic将默认转发/jsonrpc到`http://127.0.0.1:6800/jsonrpc` （aria2 rpc默认路径），可通过`--aria` 指令更改转发地址

/jsonrpc 需要被认证后才能访问（如果启用了认证的话），所以该转发是安全的，即便没有密码，其他人也无法连接到你的aria2rpc

## 文件搜索

启动后FolderMagic即开始检索被列表的文件夹并监听文件夹的所有改动。

可以在右下角菜单处打开搜索，也可使用Ctrl+F或者F3立刻开始搜索。

索引文件占用少量内存（约3M/10k文件）。监听文件夹基于inotify，如果存在海量文件夹（如十几万个）则将会占用较多内存，甚至可能用完inotify的所有监听额度，**请不要直接共享根目录。**

可以用`--nosearch`指令关闭搜索功能。如果您尝试在例如网络映射文件夹等文件系统上使用FolderMagic，索引可能会变得很慢并占用额外的资源，这时您就可以关闭搜索。

搜索功能关闭后，系统会恢复使用浏览器原生的页面内查找功能。

## 移动端适配

FolderMagic 针对移动端、触摸屏设计了适合对应设备操作的界面。只要在文件列表轻轻右划即可进入适合iOS和Android的操作界面，方便的进行上传下载和文件管理。

## 安全措施

启用鉴权后，FolderMagic将只允许授权用户登录，每个IP有5次密码试错机会，失败后将被禁用15分钟，期间FolderMagic任何服务都无法访问。

推荐使用root用户，FolderMagic将使用chroot保护运行环境，完全避免由于潜在的bug而被黑到系统其他核心文件夹的机会。

如不使用root，请开放对应用户的chroot权限。无权限时chroot将被禁用，安全性将被降低。

*由于chroot的使用，符号链接文件和符号链接文件夹可能无法使用，因为他们将指向一个完全不同的路径*

## https

FolderMagic 没有https的原生支持，你可以通过nginx或者caddy做前端来添加https的支持。

假设你的FolderMagic绑定于`127.0.0.1:81`，以下例子仅供参考：

nginx
```
server {
        listen              443 ssl;
        server_name         域名或ip;

        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
        
        #证书文件
        ssl_certificate     www.example.com.crt;
        #私钥文件
        ssl_certificate_key www.example.com.key; 
        
        #优先采取服务器算法
        ssl_prefer_server_ciphers on;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS !RC4";

		location / {
			proxy_buffering off;
			# 一定要加，否则FolderMagic在反代后不能识别客户ip，直接封锁全部用户
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://127.0.0.1:81;
		}
}
```

caddy v1 (v2 内容待添加，目前请自行google解决，本人没使用过caddy :-D）
```
https://example.com, https://www.example.com {
  gzip 
  header / {
      Strict-Transport-Security "max-age=31536000;includeSubdomains;preload"
  }
  ## HTTP 代理配置
  ### 此时访问 example.com，实际访问的是 127.0.0.1:81 的内容
  proxy / 127.0.0.1:81
  header_upstream Host {host}
  header_upstream X-Real-IP {remote}
  header_upstream X-Forwarded-For {remote}
  header_upstream X-Forwarded-Port {server_port}
  header_upstream X-Forwarded-Proto {scheme}
  tls user@example.com
}
```

## 已知问题

* 初次访问的语言将被记录，此后访问将使用第一次访问的语言。可使用`?lng=zh_CN`或`?lng=en`强制切换到中文或英文
* IE10及以下符号显示不正常，IE9及以下不能上传文件，切换文件夹将丢失当前正在预览的图像或音视频
* **360浏览器** 由于奇葩的设计，极速模式下所有文件拖放上传功能都不可用，只能在兼容模式下的IE内核才能上传
* 和部分拖放打开的插件有冲突，会出现有时能拖放有时不能，或者拖放时页面闪烁等情况，只能对本列表页面禁用插件解决。
* iOS Chrome存在一些特有问题，播放动画等会出现不流畅的情况，其他浏览器无问题，应为Chrome的自有代码缺陷。
