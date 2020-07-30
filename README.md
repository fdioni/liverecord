# ./record.sh
  ./record.sh is an automatic recording script. Support List :
  * youtube channel, 
  * twitcast channel, 
  * twitch channel, 
  * openrec channel, 
  * niconico live broadcast, niconico community, niconico channel (support login niconico account for recording), 
  * mirrativ channel, 
  * reality channel, 
  * 17live channel (18+ ( ͡° ͜ʖ ͡°)), 
  * chaturbate channel (18+ ( ͡° ͜ʖ ͡°)),
  * bilibili channel, 
  * streamlink supported live URL, 
  * ffmpeg supported m3u8 address. 
  
  Note: bilibili recording support do not record when the above channels have live broadcast, thus simply exclude the recording of retransmission; support the use of proxy recording bilibili live. 
  
  ### Feature ###
  ----
  * Support for timed segmentation. 
  * Support rclone upload, 
  * onedrive upload (including SEGi Internet version), Baidu cloud upload; 
  * support a specified number of upload error retries; 
  * support to choose whether to keep local files based on the upload results.
  
  install.sh is a one-click installation script. Currently only tested on ubuntu 18.04 and 19.10, theoretically newer linux systems should all work (centos systems should just replace apt with yum).

  record_twitcast.py is optional and is a streamlined script that can record websockets. Since twitcast provides h5 and websocket based streams respectively, but the highest clarity of some of the live streams can only be obtained via websocket, and ffmpeg does not support websocket, a script that can record websocket is provided. It can also be used separately by `python3 record_twitcast.py "ws or wss url" "output file directory"`.  

  download.sh has nothing to do with the recording function and is a completely separate little script. Essentially polling detects the live youtube channel and the first page of the video page, generates a list of youtube videos, and backs up the videos in the list with the video, cover image, title and introduction. Support to try to download the live video immediately after the download, because there is still a period of time after the live broadcast to download the full video before the deletion, and once the download begins, the download process is not affected by the deletion. Supports setting the maximum live length of the download after triggering the downlink, as live streams larger than two hours that require waiting for the download to be suppressed may not be complete. Support to create a new download after waiting for the completion of suppression while downloading to the uncompressed video, and ensure that the download to the uncompressed video. Specific usage can be achieved by running it directly without parameters `. /download.sh` to view. (Also, since the working state of the script depends entirely on the content of the video list, it might be strange to specify or modify the video list directly)

Thanks to [live-stream-recorder] (https://github.com/printempw/live-stream-recorder), [GiGaFotress/Vtuber-recorder] (https://github.com/GiGaFotress/Vtuber-recorder)  

# Installation method
### One-click installation.
`curl https://raw.githubusercontent.com/lovezzzxxx/liverecord/master/install.sh | bash`  
  * The one-click script will automatically install all of the following environment dependencies __ while overwriting the installation of the go environment and adding some environment variables __, either by annotating the corresponding commands or manually installing the environment dependencies if necessary. where record.sh and record_twitcast.py are saved in the record folder of the runtime command line directory, and livedl is saved in the livedl folder of the runtime command line directory  
  * :: The script should be called as `record/record.sh` after the one-click script is installed instead of `record.sh` as in the example below. /record.sh`__  
  * :: One-click scripts will prompt for actions that still need to be performed manually, such as updating environment variables and logging in to a web disk account, after they have been run  

### Environmental dependence
Here is a list of all the programs needed to run the auto-recorded scripts, if the one-click script installation fails or if you wish to install the environment manually you can refer to  
  * :: Auto-recording scripts, installed as `mkdir record ; wget -O "record/record.sh" "https://github.com/lovezzzxxx/liverecord/raw/master/record.sh" ; chmod +x record/record.sh`
  * [ffmpeg](https://github.com/FFmpeg/FFmpeg), installed by `sudo apt install ffmpeg`. Otherwise parameters other than youtube, twitcast, twitcastpy, nicolv, nicoco, nicoch, bilibili, bilibiliproxy are not available.
  * :: [streamlink](https://github.com/streamlink/streamlink) (based on python3), installed by `pip3 install streamlink'. Otherwise youtube, youtubeffmpeg, twitch, streamlink, 17live parameters are not available.
  * [livedl](https://github.com/himananiito/livedl) (based on go), please refer to the author's instructions on how to compile and install it __ Please place the compiled livedl file in the livedl/ folder of the runtime command line directory __. Otherwise twitcast, nicolv, nicoco, nicoch parameters cannot be used.
  * [record_twitcast.py file](https://github.com/lovezzzxxx/liverecord/blob/master/record_twitcast.py) (based on the python3 websocket library), installed by `mkdir record ; wget -O "record/record_twitcast.py" "https://github.com/lovezzzxxx/liverecord/raw/master/record_twitcast.py" ; chmod +x "record/record_twitcast.py"`, __if installed manually place the record_twitcast.py file in the record/folder of the directory where the command line is located at runtime and give executable rights__. Otherwise the twitcastpy parameter cannot be used.
  * :: [you-get](https://github.com/soimort/you-get) (based on python3), installed by `pip3 install you-get'. Otherwise bilibili, bilibiliproxy parameters cannot be used.
 The installation method is `pip3 install you-get`
  * :: [rclone](https://github.com/rclone/rclone) (supports onedrive, dropbox, googledrive, etc., and requires login), installed by `curl https://rclone.org/install.sh | sudo bash`, configured by `rclone config` and followed by instructions. Otherwise the rclone parameter cannot be used to upload.
  * :: [OneDriveUploader](https://github.com/MoeClub/OneList/tree/master/OneDriveUploader) (supports various onedrive discs, including the SEGi Internet Edition, and requires login), installation and login instructions are available at [Rat's Blog](https://www.moerats.com/archives/1006). Otherwise, you cannot upload using the onedrive parameter.
  * [BaiduPCS-Go](https://github.com/iikira/BaiduPCS-Go)(give go, support Baidu cloud disk, need to log in and use), installation and login method can be refer to the author's instructions. Otherwise the baidupan parameter cannot be used to upload.

# Method of use
### Method.
`./record.sh youtube|youtubeffmpeg|twitcast|twitcastffmpeg|twitcastpy|twitch|openrec|nicolv[:用户名,密码]|nicoco[:用户名,密码]|nicoch[:用户名,密码]|mirrativ|reality|17live|chaturbate|bilibili|bilibiliproxy[,代理ip:代理端口]|streamlink|m3u8 频道号码 [best|其他清晰度] [loop|once|视频分段时间] [10,10,1|循环检测间隔,最短录制间隔,录制开始所需连续检测开播次数] [record_video/other|其他本地目录] [nobackup|rclone:网盘名称:|onedrive|baidupan[重试次数][keep|del]] [noexcept|排除转播的youtube频道号码] [noexcept|排除转播的twitcast频道号码] [noexcept|排除转播的twitch频道号码] [noexcept|排除转播的openrec频道号码] [noexcept|排除转播的nicolv频道号码] [noexcept|排除转播的nicoco频道号码] [noexcept|排除转播的nicoch频道号码] [noexcept|排除转播的mirrativ频道号码] [noexcept|排除转播的reality频道号码] [noexcept|排除转播的17live频道号码]  [noexcept|排除转播的chaturbate频道号码] [noexcept|排除转播的streamlink支持的频道网址]`
 

### Example.
  * :: Recorded using default parameters https://www.youtube.com/channel/UCWCc8tO-uUl_7SJXIKJACMw   
`. /record.sh youtube "UCWCc8tO-uUl_7SJXIKJACMw"`  

  * Use ffmpeg to record https://www.youtube.com/channel/UCWCc8tO-uUl_7SJXIKJACMw, in turn to get the first available clarity in 1080p 720p 480p 360p worst, after the detection of live and one recording, the interval of 30 seconds to detect, the video is saved in the record_video/mea folder and automatically uploaded to the rclone after recording the name of the vps and Baidu cloud web disk the same path If the error, then retry up to three times after the end of the upload to delete the local video according to the upload situation, if the upload fails, the local video will be retained  
`. /record.sh youtubeffmpeg "UCWCc8tO-uUl_7SJXIKJACMw" 1080p,720p,480p,360p,worst once 30 "record_video/mea" rclone:vps:baidupan3`  

  * :: running in the background, using the proxy server 127.0.0.1:1080 to record https://live.bilibili.com/12235923, maximum clarity, loop detection and 7200 seconds time segment during recording, 30 seconds interval detection, minimum interval of 5 seconds from start to finish each recording, video is saved in the record_video/mea folder and automatically uploaded to the rclone after recording the name of the vps disk and the same path as onedrive and baidu cloud disk If there is an error, retry up to three times after the completion of the upload, regardless of success or failure to retain the local video, https://www.youtube.com/channel/UCWCc8tO-uUl_7SJXIKJACMw https://twitcasting.tv/kaguramea_vov do not record when there is live, log record is saved in mea_bilibili.log file  
`nohup . /record.sh bilibiliproxy,127.0.0.1:1080 "12235923" best 7200 30,5 "record_video/mea_bilibili" rclone:vps:onedrivebaidupan3keep "UCWCc8tO-uUl_7SJXIKJACMw" "kaguramea_vov" > mea_bilibili.log &`  


### Description of parameters

  * :: Required parameters, selection of recording method and corresponding channel number  

Site|First parameter|Second parameter|Explanation|Cautions
:---|:---|:---|:---|:---|:---
youtube|`youtube`, `youtubeffmpeg`|`ID part of personal home page URL` (e.g. UCWCc8tO-uUl_7SJXIKJACMw)|youtubeffmpeg is used for recording|please do not specify the third clarity parameter as best or 1080p60 and above resolution.
twitcast|`twitcast`, `twitcastffmpeg`, `twitcastpy`|` the ID part of the personal home page URL` (e.g. kaguramea_vov)|twitcastffmpeg is recorded using ffmpeg, twitcastpy is recorded using record_twitcast.py|If no dependencies are installed, only twitcast parameters can be used and twitcast maximum definition cannot be recorded. __please do not make multiple recordings of the same broadcast, it will cause file naming problems__
niconico|`nicolv`, `nicoco`, `nicoch`| are `niconico live broadcast number` (e.g. lv320447549), `niconico community number` (e.g. co41030), `niconico channel number` (e.g. macoto2525)| you can add `:username, password` to log into nico account to record (如nicolv:user@mail.com,password)| if you do not install the corresponding dependency, you cannot record niconico. Please do not use the same account for multiple recordings of the same live broadcast, it will cause websocket link conflicts and cause video jam or repeated disconnection __.
bilibili|`bilibili`、`bilibiliproxy`|`ID part`(e.g. 12235923)|bilibiliproxy is recorded by proxy, you can add `,proxy ip:proxy port` specified proxy server(e.g. bilibiliproxy,127.0.0.1:1080) directly in the back of the script, or you can add proxy acquisition method in the corresponding part of the script.
Other sites| `twitch`, `openrec`, `mirrativ`, `reality`, `17live`, `chaturbate`|` ID section of personal homepage URL`, where reality is the channel name (matching one of the channels with these words if partial name) or vlive_id (the method can be found in the script)| where twitch uses streamlink to detect live status, the system is more occupied||
Others| `streamlink`, `m3u8`| `streamlink-supported personal home page URL or live URL`, `m3u8 URL of live media stream`||

  * :: Optional parameters, __ intermediate parameters need to be filled in to specify subsequent parameters

Parameters|Functions|Defaults|Other optional values|Description
:---|:---|:---|:---|:---|:---
The third parameter, Clarity|`best`|` Clarity 1,Clarity 2`, can be separated to specify more than one clarity|Only the clarity contained in the streamlink is supported and will be tried in turn until the first available clarity is obtained.
The fourth parameter, whether or not to loop and record segmentation time|`loop`|`once` or `segmentation seconds`|is terminated when a live broadcast is detected and a recording is made if specified as once, or in loop mode if specified as a number and segmented when the corresponding number of seconds is made. __ note that there may be about ten seconds of video missing when segmenting__
The fifth parameter|Loop detection interval and shortest recording interval and the number of consecutive detection starts required for the start of recording|`10,10,1`|`seconds of loop detection interval, seconds of shortest recording interval, the number of consecutive detection starts required for the start of recording` or, if not separated by, then the shortest recording interval and the number of consecutive detection starts required for the start of recording is 1|Loop detection interval means waiting for the next detection at the corresponding time if the live broadcast is not detected; shortest recording interval means waiting for the next detection at the shortest recording interval if the distance from the start of recording is less than the shortest recording interval after a recording is over. The shortest recording interval is mainly to prevent the detection of live broadcast but recording errors, at this time, the end of one recording if immediately carry out the next detection may be too frequent detection leads to blocked IP or lead to high system occupancy, this situation may occur in special periods such as website revision, it should be noted that if a live broadcast time is too short or frequent disruptions can also trigger waiting; the number of continuous detection of the start of recording means that the corresponding number of continuous detection of the start of recording will start, can be used to prevent some detection of the live status of the actual but not live situation.
Sixth parameter|Local video repository|`record_video/other`|`Local directory`||
Seventh parameter|Whether or not to back up automatically|`nobackup`|`rclone:net disk name:` + `onedrive` + `baidupan` + `retry count` + `no/keep/del`, no spaces are needed to connect directly together (such as rclone1del or rclone:vps:onedrivebaidupan3keep)|The first three rclone, onedrive, and baidupan refer to the net disk with the corresponding name of uploading rclone, onedrive with OneDriveUploader, and Baidu PCS-Go with Baidu Cloud. The fourth item is the number of retries, which defaults to one attempt if not specified. The fifth item is whether or not to keep the local file after the upload is completed, if not specified, the local file will be deleted if the upload is successful, the local file will be kept if the upload fails, the keep parameter will always keep the local file regardless of the result, the del parameter will always delete the local file regardless of the result. If a log file without the corresponding video file is created because of an occasional detection error that causes recording to start when there is no live broadcast, the script will automatically delete the log file without the corresponding video file.
The eighth to fourteenth parameters|bilibili recording requires the exclusion of retransmissions|`noexcept`|`the corresponding channel number`, specifically the same as the second parameter, in the order youtube, twitcast, twitch, openrec, nicolv, nicoco, nicoch, mirrativ, reality, 17live, chaturbate, streamlink|bilibili only recording is valid, and bilibili recording is not performed when the corresponding channel is detected as being live
