# VideoPlayer
解决H5在微信浏览器中弹出播放框的插件

## 如何使用
使用FFMpeg转换文件ts
```sh
ffmpeg -i in.mp4 -f mpegts -codec:v mpeg1video -codec:a mp2 -b 0 out.ts
```
也可以控制视频宽高(`-s`)、帧率(`-f`)等参数，具体可以查阅ffmpeg文档。
```sh
ffmpeg -i in.mp4 -f mpegts \
	-codec:v mpeg1video -s 960x540 -b:v 1500k -r 30 -bf 0 \
	-codec:a mp2 -ar 44100 -ac 1 -b:a 128k \
	out.ts
```
引入JS核心文件
```html
<script type="text/javascript" src="jsmpeg.min.js"></script>
<script type="text/javascript" src="videoPlayer.js"></script>
 ```
实例化videoPlay
```JS
//实例化videoPlayer，传入mp4文件路径，如果是安卓微信端，会自动调用相同目录下相同文件名的.ts文件
var video = new Video("video.mp4",{
    loop : false,
    autoplay : false,
    chunkSize : 512
});
```
将实例化的video插入DOM标签
```JS
$(".video").append(video.domElement);
```

## Options

| Field           | Type            | Default  | Description                           | 
| --------------- |:---------------:| :------: | ------------------------------------  |
| `loop`          | `boolean`       | `false`  | 视频是否循环播放，该项可选。 |
| `autoplay`      | `boolean`       | `false`  | 视频是否自动播放，该项可选。          |
| `chunkSize`     | `number`        | `512`    | 设置ts视频文件分段值，单位为KB，该项只对ts文件有效，该项可选。 |
| `progressive`   | `boolean`       | `true`   | 是否分块加载数据。启用后，可以在完全加载整个源之前开始播放，该项只对ts文件有效，该项可选。 |
| `type`          | `string`        | `auto`   | 强制使用mp4文件或者ts文件，可设置的值为"mp4"、"ts"、"auto"，该项可选。 |

## Methods
实例化完成后，你可以使用以下方法进行视频播放：

| Field            | Parameter              | Description                         |
| ---------------- | :--------------------: | ----------------------------------- |
| `load()`         | none                   | loading视频，该项只对MP4文件有效，ts文件会在实例化的时候加载。   |
| `play()`         | none                   | 播放视频。 |
| `pause()`        | none                   | 暂停播放视频。|
| `stop()`         | none                   | 停止播放视频。|
| `destroy()`      | none                   | 清除视频及监听事件。|
| `paused`         | none                   | 获取视频是否暂停。 |
| `ended`          | none                   | 获取视频是否结束。 |
| `currentTime`    | none                   | 获取或设置视频的播放位置。 |
| `muted`          | none                   | 获取或设置视频是否静音。 |

## Events
播放事件的监听及取消监听的方法。

| Field                                  | Parameter           | Description           |
| -------------------------------------- | :-----------------: |---------------------- |
| `addEventListener(events,handler)`     | `events` `handler`  | 监听事件。`events` - 监听事件名称，`handler` - 监听事件执行方法。   |
| `removeEventListener(events,handler)`  | `events` `handler`  | 停止监听事件。参数同上。   |

事件监听名称：
- `"play"` - 开始播放
- `"timeupdate"` - 播放过程中
- `"pause"` - 暂停视频
- `"ended"` - 播放结束

```JS
//示例
var timeupdate = function () {
    console.log(video.currentTime);
}
video.addEventListener("timeupdate",timeupdate);
```
## Author
iorilp
