### 入门参考文章
1. [FFmpeg 视频处理入门教程 - 阮一峰的网络日志 (ruanyifeng.com)](https://ruanyifeng.com/blog/2020/01/ffmpeg.html)
2. [音频格式互转参考文章](https://www.cnblogs.com/liuyihua1992/p/9582217.html)
### 视频处理
1. mp4转yuv
```
ffmpeg -i input.mp4 -c:v rawvideo -pix_fmt yuv420p output.yuv

- c:v rawvideo：不会进行压缩
- pix_fmt yuv420p：设置yuv格式，yuv420p是常见yuv格式
- output.yuv：不含音频流
```
2. 多个YUV拼接成长的YUV
```
ffmpeg -s widthxheight -i "video1.yuv" -i "video2.yuv" -i "video3.yuv" -filter_complex concat=n=3:v=1:a=0 -f rawvideo 输出文件名.yuv

前提是：
1.这些输入文件的格式和分辨率相同。FFmpeg会自动识别文件的格式和属性
2.YUV文件只包含视频流且没有音频
```
3. yuv转y4m
```
ffmpeg -s <width>x<height> -r <framerate> -i input.yuv -pix_fmt yuv420p -f yuv4mpegpipe output.y4m
```
4. y4m转y4m
```
ffmpeg -i input.y4m -pix_fmt yuv420p output.y4m
```
5. y4m转yuv
```
ffmpeg -i input.y4m -c:v rawvideo -pix_fmt yuv420p output.yuv
或
ffmpeg -i input.y4m -f rawvideo -pix_fmt yuv420p output.yuv
```
6. 查看mp4文件元信息
```
ffprobe -i input.mp4 -select_streams v -show_streams -show_entries stream=width,height,r_frame_rate,duration
-select_streams v：表示处理视频流
-show_streams：表示显示全部流信息
-show_entries：要指定的媒体特定信息
或
ffprobe -i "{input_mp4_files[0]}" -v quiet -of csv=p=0 -select_streams v -show_entries stream=width,height,r_frame_rate
1.-v quiet表示：只输出关键信息
2.-of表示指定输出格式
3.-of csv=p=0表示指定了输出为 CSV 格式，并且使用逗号作为分隔符。p=0 表示每行输出一个逗号分隔的字段。


1. 显示输入文件的所有流信息
ffprobe -i input.mp4 -show_streams
类似于：
[STREAM] index=0 codec_name=h264 codec_type=video width=1920 height=1080 r_frame_rate=30000/1001 avg_frame_rate=30000/1001 ...
2.指定你想要处理的特定媒体流类型，后面跟着流的类型标识符（v 表示视频、a 表示音频等）
ffprobe -i input.mp4 -select_streams v -show_streams
3.指定要显示的媒体流或媒体文件的特定信息
ffprobe -i input.mp4 -show_entries stream=codec_name,width,height
```
### 音频处理
1. 提取视频中的音频
```
ffmpeg -i input.mp4 -vn -c:a copy output.aac

-vn：表示去掉视频
-c:a copy：表示不改变音频编码，直接拷贝
```
2. aac转MP3
```
ffmpeg -i input.aac -acodec libmp3lame -ac 2 -ab 160 output.mp3

-acodec：音频编码格式，libmp3lame是MP3编码格式
-ac 设置通道
-ab 设置音频码率
-ar 设置音频采样率
```
3. 调整MP3音量
```
ffmpeg -i input.mp3 -filter: "volume = 5dB" output.mp3

- 5dB是增加，负数则是减小


或

ffmpeg -i input.mp3 -filter: "volume = 2" output.mp3

- 2为音频音量变大一倍，标准不变是1，0.5就是减小一倍音量
```
4. 合并多个MP3音频
```
ffmpeg -i input1.mp3 -i input2.mp3 -filter_complex amix=inputs=2:duration=first:dropout_transition=2 -f mp3 remix.mp3

-filter_complex：滤镜功能
amix：混合多音频为单音频
inputs：表示2个音频文件
duration：最终文件长度
```