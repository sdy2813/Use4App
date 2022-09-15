# ffmpeg 基本操作

## 查看音视频文件的流信息
```
ffprobe -show_streams inputFile
```
## 视频转音频
```
# 把MP4格式的文件转换成 128kbps的MP3格式
ffmpeg -i input.MP4 -ab 128k output.mp3

# -codec的意思是直接复制流。
ffmpeg -i input.mp4 -vn -codec copy out.m4a
```

## 更改视频文件的分辨率
```
ffmpeg -i input.mp4 -s 1280x720 -c:a copy output.mp4
```
## 压缩视频文件
```
ffmpeg -i input.mp4 -vf scale=1280:-1 -c:v libx264 -preset veryslow -crf 24 output.mp4
```
## 视频格式转换
```
ffmpeg -i video.flv video.mpg

ffmpeg -i test.mp4 -ab 56 -ar 22050 -qmin 2 -qmax 16 -b 320k -r 15 -s 320x240 outputfile.flv     #mp4 转 flv  
      
ffmpeg -i outputfile.flv -copyts -strict -2 test.mp4       #flv 转 mp4  
for i in *.wav;do ffmpeg -i "$i" -f mp3 "${i}.mp3";done
```

## 视频裁剪
```
#这个例子是将test.mp4视频的前3秒，重新生成一个新视频。

#-ss 开始时间，如： 00:00:00，表示从0秒开始，格式也可以00:00:0

#-t 时长，如： 00:00:03，表示截取3秒长的视频，格式也可以00:00:3

#-y 如果文件已存在强制替换；

#-i 输入，后面是空格，紧跟着就是输入视频文件；

#-vcodec copy 和 -acodec copy表示所要使用的视频和音频的编码格式，这里指定为copy表示原样拷贝；

ffmpeg -ss 00:00:00 -t 00:00:03 -y -i test.mp4 -vcodec copy -acodec copy test1.mp4

```

## 视频合并
```
vi join.txt
file '12.mp4'
ffmpeg -f concat -i filelist.txt -c copy output.mp4
```
## 获取视频时长
```
ffmpeg -i test.mp4 2>&1 | grep 'Duration' | cut -d ' ' -f 4 | sed s/,//  
```

## 从一个视频文件移除音频流
```
ffmpeg -i input.mp4 -an output.mp4
```
## 从一个媒体文件移除视频流
```
ffmpeg -i input.mp4 -vn output.mp3
```
## 音频+视频=视频
```
ffmpeg -i audio.mp3 -i video.avi video_audio_mix.mpg

ffmpeg -r 25 -loop 1 -i 1.jpg -pix_fmt yuv420p -vcodec libx264 -b:v 600k -r:v 25 -preset medium -crf 30 -s 720x576 -vframes 250 -r 25 -t 180 a.mp4
```

```
ffmpeg -framerate 30 -i input.jpg -t 15 \
    -c:v libx265 -x265-params lossless=1 \
    -pix_fmt yuv420p -vf "scale=3840:2160,loop=-1:1" \
    -movflags faststart \
    out.mp4
 
-framerate 30 rather than default 25 fps
-t 15 video duration 15 seconds
-c:v libx265 encode using h.265 (HEVC)
-x265-params lossless=1 lossless encoding, prevent artifacts for a still image which will be displayed for long enough to notice them
-pix_fmt yuv420p set pixel format for compatibility when input is an image (JPEGs are often YUV444, which many decoders will not accept)
-vf "<options>" sets video filter
scale=3840:2160 specifically scale to 3840x2160 (4K)
You will want to use 3840:-1 or -1:2160 if your input is not exactly 16:9 (or whatever your target ratio is). This will prevent significant blurring due to an unintentional aspect ratio change. Look into pad instead, if you must have an exact resoluation that does not match the ratio of the input.
loop=-1:1 infinitely loop (-1) for groups of 1 frame (1)
movflags faststart will put metadata at front of file for faster playback in web browsers.
I am running this in a cloud/serverless environment, so minimizing runtime is key. I tried the low framerate approach, but it did not play in my target environment or locally on VLC.

The -vf loop option is what loops the one frame here. It keeps that frame in memory and is the main reason why this is faster than other answers, while being more compatible than a fractional framerate approach.
```

```
ffmpeg \
-loop 1 -t 5 -i 空城计_01.png \
-loop 1 -t 177 -i 空城计_02.png \
-loop 1 -t 60 -i 空城计_03.png \
-loop 1 -t 79 -i 空城计_04.png \
-loop 1 -t 54 -i 空城计_05.png \
-i 空城计.mp3 \
-filter_complex \
"[0:v]scale=1280:720:force_original_aspect_ratio=decrease,pad=1280:720:(ow-iw)/2:(oh-ih)/2,setsar=1,fade=t=out:st=4:d=1[v0]; \
 [1:v]scale=1280:720:force_original_aspect_ratio=decrease,pad=1280:720:(ow-iw)/2:(oh-ih)/2,setsar=1,fade=t=in:st=0:d=1,fade=t=out:st=176:d=1[v1]; \
 [2:v]scale=1280:720:force_original_aspect_ratio=decrease,pad=1280:720:(ow-iw)/2:(oh-ih)/2,setsar=1,fade=t=in:st=0:d=1,fade=t=out:st=60:d=1[v2]; \
 [3:v]scale=1280:720:force_original_aspect_ratio=decrease,pad=1280:720:(ow-iw)/2:(oh-ih)/2,setsar=1,fade=t=in:st=0:d=1,fade=t=out:st=79:d=1[v3]; \
 [4:v]scale=1280:720:force_original_aspect_ratio=decrease,pad=1280:720:(ow-iw)/2:(oh-ih)/2,setsar=1,fade=t=in:st=0:d=1,fade=t=out:st=54:d=1[v4]; \
 [v0][v1][v2][v3][v4]concat=n=5:v=1:a=0,format=yuv420p[v]" -map "[v]" -map 5:a -shortest kcj_test.mp4

```

---
参考资料
1. [给新手的 20 多个 FFmpeg 命令示例](https://zhuanlan.zhihu.com/p/67878761)

1. [FFmpeg 视频处理入门教程](https://www.ruanyifeng.com/blog/2020/01/ffmpeg.html)

1. [学习ffmpeg，整理资料编写技术手册](https://github.com/feixiao/ffmpeg)

1. [Creating a video from a single image for a specific duration in ffmpeg](https://stackoverflow.com/questions/25891342/creating-a-video-from-a-single-image-for-a-specific-duration-in-ffmpeg)

1. [How to use FFmpeg to convert images to video](https://shotstack.io/learn/use-ffmpeg-to-convert-images-to-video/)
