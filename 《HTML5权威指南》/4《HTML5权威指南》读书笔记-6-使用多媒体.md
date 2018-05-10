# 1、使用video元素

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>video</title>
</head>
<body>
	<video width="360" height="240" src="timessquare.webm" autoplay controls preload="none" muted>
		Video cannot be displayed
	</video>
</body>
</html>
```
如果浏览器不支持video元素或者无法播放视频，那么备用内容就会代替显示。

以下是video元素：

    autoplay          如果存在，此属性会使浏览器尽可能立刻开始播放视频
    preload           告诉浏览器是否要预先载入视频（详情见后）
    controls          除非此属性存在，否则浏览器不会显示播放控件
    loop              如果存在，此属性会让浏览器反复播放视频。
    poster            指定在视频数据载入时显示的图片（详情见后）
    height  width     设置高宽
    muted             如果此属性存在，视频从一开始就会处于静音状态。
    src               指定显示视频来源（详情见后）
## 1.1预先加载视频
preload属性告诉浏览器：当它加载完包含video元素的网页后，是否应该积极地去下载视频。这样预先加载视频减少了用户播放时的初始延迟，但是如果用户不观看视频则会造成网络宽带的浪费。

这个属性允许的值：

    none      用户开始播放之前不会载入视频。
    metadata  用户开始播放之前只能载入视频的元数据（宽度、高度、第一帧、长度和其他此类信息）
    auto      请求浏览器尽快下载整个视频。浏览器可以忽略这个请求。这是默认行为。
这个功能自动载入视频会带来更平滑的用户体验，但它可能会大大提升经营成本，如果用户没有观看视频就离开网页，那么这些成本就浪费了。

这个属性的metadata值可以被用来在none和auto值之间建立起适度的平衡。none值的问题在于视频内容会在屏幕上显示为一片空白区域，metadata会让浏览器获取足够的信息来向用户展示视频的第一帧，而不必下载全部内容。