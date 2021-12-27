# ffmpeg 基本操作

## 查看音视频文件的流信息
ffprobe -show_streams inputFile

## 视频转音频
···
# 把MP4格式的文件转换成 128kbps的MP3格式
ffmpeg -i input.MP4 -ab 128 output.mp3
···

## 更改视频文件的分辨率
ffmpeg -i input.mp4 -s 1280x720 -c:a copy output.mp4

## 压缩视频文件
ffmpeg -i input.mp4 -vf scale=1280:-1 -c:v libx264 -preset veryslow -crf 24 output.mp4

## 视频格式转换
···
ffmpeg -i video.flv video.mpg

ffmpeg -i test.mp4 -ab 56 -ar 22050 -qmin 2 -qmax 16 -b 320k -r 15 -s 320x240 outputfile.flv     #mp4 转 flv  
      
ffmpeg -i outputfile.flv -copyts -strict -2 test.mp4       #flv 转 mp4  
for i in *.wav;do ffmpeg -i "$i" -f mp3 "${i}.mp3";done
···

##视频裁剪
···
#这个例子是将test.mp4视频的前3秒，重新生成一个新视频。

#-ss 开始时间，如： 00:00:00，表示从0秒开始，格式也可以00:00:0

#-t 时长，如： 00:00:03，表示截取3秒长的视频，格式也可以00:00:3

#-y 如果文件已存在强制替换；

#-i 输入，后面是空格，紧跟着就是输入视频文件；

#-vcodec copy 和 -acodec copy表示所要使用的视频和音频的编码格式，这里指定为copy表示原样拷贝；

ffmpeg -ss 00:00:00 -t 00:00:03 -y -i test.mp4 -vcodec copy -acodec copy test1.mp4

···

## 视频合并
vi join.txt
file '12.mp4'
ffmpeg -f concat -i filelist.txt -c copy output.mp4

## 获取视频时长
···
ffmpeg -i test.mp4 2>&1 | grep 'Duration' | cut -d ' ' -f 4 | sed s/,//  
···

## 从一个视频文件移除音频流
ffmpeg -i input.mp4 -an output.mp4
 
## 从一个媒体文件移除视频流
ffmpeg -i input.mp4 -vn output.mp3

## 音频+视频=视频
ffmpeg -i audio.mp3 -i video.avi video_audio_mix.mpg

ffmpeg -r 25 -loop 1 -i 1.jpg -pix_fmt yuv420p -vcodec libx264 -b:v 600k -r:v 25 -preset medium -crf 30 -s 720x576 -vframes 250 -r 25 -t 180 a.mp4


---
参考资料
1. [给新手的 20 多个 FFmpeg 命令示例](https://zhuanlan.zhihu.com/p/67878761)

1. [FFmpeg 视频处理入门教程](https://www.ruanyifeng.com/blog/2020/01/ffmpeg.html)

1. [学习ffmpeg，整理资料编写技术手册](https://github.com/feixiao/ffmpeg)


