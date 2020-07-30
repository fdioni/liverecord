# 功能介绍
  * record.sh为自动录播脚本。支持youtube频道、twitcast频道、twitch频道、openrec频道、niconico生放送、niconico社区、niconico频道（支持登录niconico账号进行录制）、mirrativ频道、reality频道、17live频道、chaturbate频道、bilibili频道、streamlink支持的直播网址、ffmpeg支持的m3u8地址。bilibili录制支持在上述频道有直播时不进行录制，从而简单的排除转播的录制；支持使用代理录制bilibili直播。支持定时分段。支持rclone上传、onedrive上传(含世纪互联版)、百度云上传；支持可指定次数的上传出错重试；支持根据上传结果选择是否保留本地文件。
  
  * install.sh为一键安装脚本。目前仅在ubuntu18.04与19.10系统测试过，理论上较新的linux系统应该都可以使用(centos系统应该把apt替换为yum就行了)。

  * record_twitcast.py为可选，是一个可以录制websocket的精简脚本。因为twitcast分别提供了基于h5与websocket的流，但部分直播的最高清晰度仅能通过websocket获取，而ffmpeg并不能支持websocket，所以提供一个可以录制websocket的脚本。也可单独使用，方法为`python3 record_twitcast.py "ws或wss网址" "输出文件目录"`。  

  * download.sh与录制功能无关，是一个完全独立的小脚本。本质是轮询检测youtube频道的直播和视频页第一页，产生一个youtube视频列表，并对列表中视频的录像、封面图、标题与简介进行备份。支持尝试在下播后立即下载直播录像，因为直播结束后到删档前仍有一段时间可以下载完整的录像，而且一旦开始下载则下载过程不受删档影响。支持设置触发下播后下载的最长直播时长，因为大于两个小时的直播需要等待压制下载的录像可能不全。支持在下载到未压制完成的录像的情况下等待压制完成后建立新的下载，确保下载到压制完成的未删档视频。具体用法可直接不加参数运行`./download.sh`查看。（另外由于脚本的工作状态完全取决于视频列表的内容，所以直接指定或修改视频列表大概会有奇怪的作用）

感谢[live-stream-recorder](https://github.com/printempw/live-stream-recorder)、[GiGaFotress/Vtuber-recorder](https://github.com/GiGaFotress/Vtuber-recorder)  

# 安装方法
### 一键安装
`curl https://raw.githubusercontent.com/lovezzzxxx/liverecord/master/install.sh | bash`  
  * 一键脚本将会自动安装下列所有环境依赖， __同时会覆盖安装go环境并添加一些环境变量__ ，如果有需要可以注释掉相应的命令或者手动安装环境依赖。其中record.sh和record_twitcast.py会保存于运行时命令行所在目录的record文件夹下，livedl会保存于运行时命令行所在目录的livedl文件夹下  
  * __一键脚本安装后脚本调用方式应为`record/record.sh`而非下文示例中的`./record.sh`__  
  * 一键脚本运行结束后会提示仍需要手动进行的操作，如更新环境变量和登录网盘账号  

### 环境依赖
此处列举自动录播脚本运行所需的所有程序，如果一键脚本安装失败或希望手动安装环境可以参考  
  * 自动录播脚本，安装方法为`mkdir record ; wget -O "record/record.sh" "https://github.com/lovezzzxxx/liverecord/raw/master/record.sh" ; chmod +x record/record.sh`
  * [ffmpeg](https://github.com/FFmpeg/FFmpeg)，安装方法为`sudo apt install ffmpeg`。否则无法使用除了youtube、twitcast、twitcastpy、nicolv、nicoco、nicoch、bilibili、bilibiliproxy以外的参数。
  * [streamlink](https://github.com/streamlink/streamlink)(基于python3)，安装方法为`pip3 install streamlink`。否则无法使用youtube、youtubeffmpeg、twitch、streamlink、17live参数。
  * [livedl](https://github.com/railannad/livedl)(基于go，原项目[himananiito/livedl](https://github.com/himananiito/livedl)已失效)，具体编译安装方法可以参考作者的说明， __请将编译完成的livedl文件放置于运行时命令行所在目录的livedl/文件夹内__ 。否则无法使用twitcast、nicolv、nicoco、nicoch参数。
  * [record_twitcast.py文件](https://github.com/lovezzzxxx/liverecord/blob/master/record_twitcast.py)(基于python3 websocket库)，安装方法为`mkdir record ; wget -O "record/record_twitcast.py" "https://github.com/lovezzzxxx/liverecord/raw/master/record_twitcast.py" ; chmod +x "record/record_twitcast.py"`， __如果手动安装请将record_twitcast.py文件放置于运行时命令行所在目录的record/文件夹内并给予可执行权限即可__ 。否则无法使用twitcastpy参数。
  * [you-get](https://github.com/soimort/you-get)(基于python3)，安装方法为`pip3 install you-get`。否则无法使用bilibili、bilibiliproxy参数。
 安装方法为`pip3 install you-get`
  * [rclone](https://github.com/rclone/rclone)(支持onedrive、dropbox、googledrive等多种网盘，需登录后使用)，安装方法为`curl https://rclone.org/install.sh | sudo bash`，配置方法为`rclone config`后根据说明进行。否则无法使用rclone参数上传。
  * [OneDriveUploader](https://github.com/MoeClub/OneList/tree/master/OneDriveUploader)(支持包括世纪互联版在内的各种onedrive网盘，需登录后使用)，安装和登录方法可以参考[Rat's Blog](https://www.moerats.com/archives/1006)。否则无法使用onedrive参数上传。
  * [BaiduPCS-Go](https://github.com/felixonmars/BaiduPCS-Go)(给予go，支持百度云网盘，需登录后使用，原项目[iikira/BaiduPCS-Go](https://github.com/iikira/BaiduPCS-Go)已失效)，安装和登录方法可以参考作者的说明。否则无法使用baidupan参数上传。

# 使用方法
### 方法
`./record.sh youtube|youtubeffmpeg|twitcast|twitcastffmpeg|twitcastpy|twitch|openrec|nicolv[:用户名,密码]|nicoco[:用户名,密码]|nicoch[:用户名,密码]|mirrativ|reality|17live|chaturbate|bilibili|bilibiliproxy[,代理ip:代理端口]|streamlink|m3u8 频道号码 [best|其他清晰度] [loop|once|视频分段时间] [10,10,1|循环检测间隔,最短录制间隔,录制开始所需连续检测开播次数] [record_video/other|其他本地目录] [nobackup|rclone:网盘名称:|onedrive|baidupan[重试次数][keep|del]] [noexcept|排除转播的youtube频道号码] [noexcept|排除转播的twitcast频道号码] [noexcept|排除转播的twitch频道号码] [noexcept|排除转播的openrec频道号码] [noexcept|排除转播的nicolv频道号码] [noexcept|排除转播的nicoco频道号码] [noexcept|排除转播的nicoch频道号码] [noexcept|排除转播的mirrativ频道号码] [noexcept|排除转播的reality频道号码] [noexcept|排除转播的17live频道号码]  [noexcept|排除转播的chaturbate频道号码] [noexcept|排除转播的streamlink支持的频道网址]`

### 示例
  * 使用默认参数录制https://www.youtube.com/channel/UCWCc8tO-uUl_7SJXIKJACMw   
`./record.sh youtube "UCWCc8tO-uUl_7SJXIKJACMw"`  

  * 使用ffmpeg录制https://www.youtube.com/channel/UCWCc8tO-uUl_7SJXIKJACMw ，依次获取1080p 720p 480p 360p worst中第一个可用的清晰度，在检测到直播并进行一次录制后终止，间隔30秒检测，录像保存于record_video/mea文件夹中并在录制完成后自动上传到rclone中名称为vps的网盘和百度云网盘的相同路径 如果出错则重试最多三次 上传结束后根据上传情况删除本地录像，如果上传失败则会保留本地录像  
`./record.sh youtubeffmpeg "UCWCc8tO-uUl_7SJXIKJACMw" 1080p,720p,480p,360p,worst once 30 "record_video/mea" rclone:vps:baidupan3`  

  * 后台运行，使用代理服务器127.0.0.1:1080录制https://live.bilibili.com/12235923 ，最高清晰度，循环检测并在录制进行7200秒时分段，间隔30秒检测 每次录制从开始到结束最短间隔5秒，录像保存于record_video/mea文件夹中并在录制完成后自动上传到rclone中名称为vps的网盘和onedrive和百度云网盘的相同路径 如果出错则重试最多三次 上传完成后无论成功与否都保留本地录像，在https://www.youtube.com/channel/UCWCc8tO-uUl_7SJXIKJACMw https://twitcasting.tv/kaguramea_vov 有直播时不进行录制，log记录保存于mea_bilibili.log文件  
`nohup ./record.sh bilibiliproxy,127.0.0.1:1080 "12235923" best 7200 30,5 "record_video/mea_bilibili" rclone:vps:onedrivebaidupan3keep "UCWCc8tO-uUl_7SJXIKJACMw" "kaguramea_vov" > mea_bilibili.log &`  


### 参数说明

  * 必选参数，选择录制方式与相应频道号码  

网站|第一个参数|第二个参数|说明|注意事项
:---|:---|:---|:---|:---
youtube|`youtube`、`youtubeffmpeg`|`个人主页网址中的ID部分`(如UCWCc8tO-uUl_7SJXIKJACMw)|youtubeffmpeg为使用ffmpeg进行录制|请不要将第三个清晰度参数指定为best或1080p60及以上的分辨率
twitcast|`twitcast`、`twitcastffmpeg`、`twitcastpy`|`个人主页网址中的ID部分`(如kaguramea_vov)|twitcastffmpeg为使用ffmpeg进行录制，twitcastpy为使用record_twitcast.py进行录制|如果未安装相应依赖，则仅能使用twitcast参数，无法录制twitcast最高清晰度。 __请不要对同一场直播进行多个录制，会导致文件命名问题__
niconico|`nicolv`、`nicoco`、`nicoch`|分别为`niconico生放送号码`(如lv320447549)，`niconico社区号码`(如co41030)，`niconico频道号码`(如macoto2525)|可以在后方添加`:用户名,密码`来登录nico账号进行录制(如nicolv:user@mail.com,password)|如果未安装相应依赖，则无法录制niconico。 __请不要对同一场直播使用同一账号进行多个录制，会产生websocket链接冲突导致录像卡顿或反复断连__
bilibili|`bilibili`、`bilibiliproxy`|`直播间网址中的ID部分`(如12235923)|bilibiliproxy为通过代理进行录制，可以直接在后方添加`,代理ip:代理端口`指定代理服务器(如bilibiliproxy,127.0.0.1:1080)，也可以在脚本内相应部分添加代理获取方法
其他网站| `twitch`、`openrec`、`mirrativ`、`reality`、`17live`、`chaturbate`|`个人主页网址中的ID部分`，其中reality为频道名称(如果为部分名字则匹配含有这些文字的其中一个频道)或vlive_id(获取方法可于脚本内查找)|其中twitch使用streamlink检测直播状态，系统占用较高||
其他|`streamlink`、`m3u8`|`streamlink支持的个人主页网址或直播网址`、`直播媒体流的m3u8网址`||

  * 可选参数， __需要补全中间的参数才能指定后续的参数__

参数|功能|默认值|其他可选值|说明
:---|:---|:---|:---|:---
第三个参数|清晰度|`best`|`清晰度1,清晰度2`，可以用,分隔来指定多个清晰度|仅支持streamlink含有的清晰度，将会依次获取尝试直到获取第一个可用的清晰度
第四个参数|是否循环和录制分段时间|`loop`|`once`或`分段秒数`|如果指定为once则会在检测到直播并进行一次录制后终止，如果指定为数字则会以loop模式进行录制并在在录制进行相应秒数时分段。 __注意分段时可能会有十秒左右的视频缺失__
第五个参数|循环检测间隔和最短录制间隔和录制开始所需连续检测开播次数|`10,10,1`|`循环检测间隔秒数,最短录制间隔秒数,录制开始所需连续检测开播次数`，如果不以,分隔则最短录制间隔也为此值而录制开始所需连续检测开播次数为1|循环检测间隔是指如果未检测到直播，则等待相应时间进行下一次检测；最短录制间隔是指如果一次录制结束后，如果距离录制开始小于最短录制间隔，则等待到最短录制间隔进行下一次检测。最短录制间隔主要是为了防止检测到直播但录制出错的情况，此时一次录制结束如果立即进行下一次检测可能会因为检测过于频繁导致被封禁IP或者导致高系统占用，这种情况可能出现在网站改版等特殊时期，需要注意的是如果一次直播时间过短或者频繁断流也能触发等待；录制开始所需连续检测开播次数是指需要连续检测到相应次数的开播才会开始录制，可以用于预防一些检测到直播状态实际却并没有直播的情况。
第六个参数|本地录像存放目录|`record_video/other`|`本地目录`||
第七个参数|是否自动备份|`nobackup`|`rclone:网盘名称:` + `onedrive` + `baidupan` + `重试次数` + `无/keep/del`，不需要空格直接连接在一起即可(如rclone1del或rclone:vps:onedrivebaidupan3keep)|其中前三项的rclone、onedrive、baidupan分别指上传rclone相应名称的网盘、OneDriveUploader登录的onedrive网盘、BaiduPCS-Go登录的百度云网盘。第四项为重试次数，如果不指定则默认为尝试一次。第五项为上传完成后是否保留本地文件，如果不指定则上传成功将删除本地文件，上传失败将保留本地文件，keep参数为不论结果始终保留本地文件，del参数为不论结果始终删除本地文件。如果因为偶发的检测异常导致没有直播时开始录制，进而产生没有相应录像文件的log文件，脚本将会自动删除这个没有对应录像文件的log文件
第八至十四个参数|bilibili的录制需要排除的转播|`noexcept`|`相应频道号码`，具体同第二个参数，顺序分别为youtube、twitcast、twitch、openrec、nicolv、nicoco、nicoch、mirrativ、reality、17live、chaturbate、streamlink|仅bilibili录制有效，检测到相应频道正在直播时不进行bilibili的录制
