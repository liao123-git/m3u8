# m3u8
解决视频过大问题   mp4 切片成 m3u8

### 切片
下载命令行切片工具
官方下载网站: http://www.ffmpeg.org/download.html#build-windows
官网如果上不去可以到这里下载:https://download.csdn.net/download/qq_36592993/18206572
- cmd进入到文件目录\bin下
- 执行切片命令`ffmpeg -i D:\xxx\test.mp4 -c:v libx264 -c:a aac -strict -2 -f hls -hls_list_size 0 D:\xxx\xxx\xxx.m3u8`

### `Vue-cli` 中播放 `m3u8` 文件
- 安装`video.js`
	- `npm i video.js`
	- 官网地址：https://videojs.com/
- 安装`videojs-contrib-hls`
	- `npm i videojs-contrib-hls`
	- 为了让`video.js`能播放`m3u8`类型文件
- 修改`xxx.vue`
	- 在`template`里加入
	```html
	<video id="myVideoPlayer" class="video-js vjs-big-play-centered" muted></video>
	```
	- `id`：`video.js` 需要通过`id`来找到`video`标签
	- `class`
		- 'video-js'：使用`video.js`默认的样式
		- 'vjs-big-play-centered'：让播放按钮居中
		- 具体可以看`video.js`官网
	- 在`script`里导入插件
	```js
	import "video.js/dist/video-js.css"
	import videojs from "video.js";
	import 'videojs-contrib-hls';
	```
	- 修改`vue`的`data`
	```js
	data () {
		return {
			player: null, // 存放 video.js 的实例
			nowPlayVideoUrl: "https://qg.aiddl.cn/source/m3u8/cjjs.m3u8" // m3u8 的地址，不要放到项目里，webpack 无法解析此类文件
		}
	}
	```
	- 修改`vue`的`methods`
	```js
	methods: {
		initVideo (nowPlayVideoUrl) {
			  const options = {
				autoplay: false, // 设置自动播放
				controls: true, // 显示播放的控件
				preload: 'auto', // 默认加载视频，需要浏览器支持
				sources: [
				  // 注意，如果是以option方式设置的src,是不能实现 换台的 (即使监听了nowPlayVideoUrl也没实现)
				  {
					src: nowPlayVideoUrl,
					type: "application/x-mpegURL" // 告诉videojs,这是一个hls流
				  }
				]
			  };
			  // videojs的第一个参数表示的是，文档中video的id
			  this.player = videojs("myVideoPlayer", options);
    	}
	}
	```
	- 修改`vue`的`mounted`
	```js
	methods: {
		// 需要下一帧才能获取到 dom
		this.$nextTick(() => this.initVideo(this.nowPlayVideoUrl))
	}
	```
	- 修改`vue`的`beforeDestroy`
	```js
	beforeDestroy () {
		if ( this.player ) {
			//	如果视频已经在加载了，就销毁
			this.player.dispose();
		}
	}
	```
