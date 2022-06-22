# m3u8
解决视频过大问题   mp4 切片成 m3u8

### 切片
官方下载网站: http://www.ffmpeg.org/download.html#build-windows
官网如果上不去可以到这里下载:https://download.csdn.net/download/qq_36592993/18206572
- cmd进入到文件目录\bin下
- 执行切片命令 ```fmpeg -i D:\xxx\test.mp4 -c:v libx264 -c:a aac -strict -2 -f hls -hls_list_size 0 D:\xxx\xxx\xxx.m3u8```

### Vue-cli 中播放 m3u8 文件
