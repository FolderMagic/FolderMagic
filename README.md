FolderMagic
----------

简单易用，无需部署的列表程序。
特性：
* 无需环境，无需数据库，低内存占用
* 支持webdav管理
* 支持视频在线播放，支持字幕（srt, ass, ssa, vtt等）
* 支持图片预览，支持常见jpg, gif, png, tif, psd格式预览
* 支持音频在线播放，支持解析专辑图片和歌手信息
* 支持文档在线预览，包括常见各类代码格式，如html, js, css, php, py, pdf等
* 支持office在线预览
* 支持共享链接，支持共享链接管理
* 集成aria2ng，并安全的指向内部转发地址
* 文件搜索，即时搜索整个列表文件夹
* 中英多语言支持
* 支持常用文件管理，文件一拖即传
* 支持主流浏览器，完整支持IE11，部分支持IE10和IE9

##命令行参数
```
  -aria string
        Aria2 RPC地址 (默认 "http://127.0.0.1:6800/jsonrpc")，列表程序将安全的转发这个地址
  -auth string
        认证: "用户名:密码" 认证信息用于网页登录和webdav，不设置则无认证，webdav将被禁用
  -bind string
        监听端口 (默认 ":80")
  -gzip
        使用gzip压缩 (默认 true)
  -page404 string
        自定义404页面
  -root string
        列表根目录 (默认为当前目录)
  -share int
        默认共享链接有效期，单位分钟 (默认 60)
  -wd string
        用于webdav的认证路径, 不可使用根目录 (默认 "/manager")
```

##webdav 使用

使用命令行 `-auth user:password` 启用鉴权后webdav即自动启用。
使用raidrive或其他webdav兼容客户端连接 http://your.domain:port/manager 输入用户名和密码即可连接。
/manager 可使用 `-wd` 指令更改

##文件管理

在网页列表界面右键即可操作文件和文件夹，可以新建和删除。
不使用认证时只能下载文件，不能进行其他操作

##文件上传

1. 使用webdav客户端上传。

2. 直接把文件拖到列表界面，出现蓝框提示松手上传即可。
上传过程中可继续拖入新的文件排队上传。不支持拖文件夹上传。
IE9 及以下浏览器由于浏览器限制无法上传。

##视频预览

支持预览mp4, mkv格式，能否成功播放取决于实际视频容器内的编码格式。
字幕需要在视频同一文件夹下，字幕文件名包含视频名称即可，如`a.mp4`和`a_en.srt`即为匹配字幕。多个字幕将被同时载入可以在播放界面选择。字幕支持所有常见字幕，ass特效字幕保留文字部分，特效无法支持。

##音频预览

支持mp3和ogg，具体支持视浏览器而定。点击即可在左下角开始播放。
音频将被自动解析专辑图片和歌曲名称，歌手。（IE9不支持解析）
点击新的音频可以自动加入播放列表，切换文件夹不影响播放。 （IE9 将中断播放）

##图片预览

支持各种常见图片预览，图片将自动生成合适缩略图并使用webp格式（如果浏览器支持）传输
支持psd格式预览，gif格式生成缩略图后没有动画。

##Office预览

基于微软在线预览实现。需要将拥有域名并且FolderMagic需要在80或443端口，否则无法预览。

##共享管理

通过右键复制的临时链接自动拥有一定时间的有效期（默认60分钟，可通过`-share`指令更改），到期后无法被下载。
在右下角菜单中选择共享管理即可添加或减少共享时间，也可删除共享
复制的永久链接除非移动文件或更改用户名密码，否则永远有效，不可删除。
重启FolderMagic后，所有临时共享都会失效，永久连接依然有效

##AriaNG

通过右下角菜单可以调用内置的ariaNg，并默认指向/jsonrpc路径。FolderMagic将默认转发/jsonrpc到`http://127.0.0.1:6800/jsonrpc` （aria2 rpc默认路径），可通过`-aria` 指令更改转发地址
/jsonrpc 需要被认证后才能访问（如果启用了认证的话），所以该转发是安全的，即便没有密码，其他人也无法连接到你的aria2rpc

##文件搜索

启动后FolderMagic即开始检索被列表的文件夹并监听文件夹的所有改动。
可以在右下角菜单处打开搜索，也可使用Ctrl+F或者F3立刻开始搜索。
索引文件占用少量内存（约3M/10k文件）。监听文件夹基于inotify，如果存在海量文件夹（如十几万个）则将会占用较多内存，甚至可能用完inotify的所有监听额度，**请不要直接共享根目录。**

##安全措施

启用鉴权后，FolderMagic将只允许授权用户登录，每个IP有5次密码试错机会，失败后将被禁用15分钟，期间FolderMagic任何服务都无法访问。
推荐使用root用户，FolderMagic将使用chroot保护运行环境，完全避免由于潜在的bug而被黑到系统其他核心文件夹的机会。
如不使用root，请开放对应用户的chroot权限。无权限时chroot将被禁用，安全性将被降低。
*由于chroot的使用，符号链接文件和符号链接文件夹可能无法使用，因为他们将指向一个完全不同的路径*

##https

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
			proxy_pass http://127.0.0.1:81;
		}
}
```

caddy
```
https://example.com, https://www.example.com {
  gzip 
  header / {
      Strict-Transport-Security "max-age=31536000;includeSubdomains;preload"
  }
  ## HTTP 代理配置
  ### 此时访问 example.com，实际访问的是 127.0.0.1:81/ 的内容
  proxy / 127.0.0.1:81
  tls user@example.com
}

```

##已知问题

* 初次访问的语言将被记录，此后访问将使用第一次访问的语言。可使用`?lng=zh_CN`或`?lng=en`强制切换到中文或英文
* IE10及以下符号显示不正常，IE9及以下不能上传文件，切换文件夹将丢失当前正在预览的图像或音视频