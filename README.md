@[toc](html5调用摄像头并拍照)

# 一、注意

- 1、<font color="red">必须在https协议下才</font>，并且允许实用摄像头。
- 2、原本是想在手机上用的，但发现ios手机上不行，安卓的不知道，pc+chrome就可以，待解决。

# 二、开始
研究了下用html5+javascript 调用摄像头并拍照，代码参考了网上的，然后做了个小demo。

原理主要用到 Vedio + Canvas + getUserMedia 。

有兴趣的可以看看这里：[http://blog.csdn.net/theforever/article/details/8251671](http://blog.csdn.net/theforever/article/details/8251671)

**效果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191015213828901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2lhbWx1amluZ3Rhbw==,size_16,color_FFFFFF,t_70)
# 三、在线演示和源码

[-->在线演示地址](https://lujingtao.github.io/html5-camera-demo/)
[-->GitHub源码](https://github.com/lujingtao/html5-camera-demo)


**核心代码：**
```js
	//判断浏览器是否支持HTML5 Canvas
	window.onload = function () {
	try {                   
		//动态创建一个canvas元 ，并获取他2Dcontext。如果出现异常则表示不支持 document.createElement("canvas").getContext("2d");        
		document.getElementById("support").innerHTML = "浏览器支持HTML5 CANVAS";
		takePhotos();		
	}
	catch (e) {           
		document.getElementById("support").innerHTML = "浏览器不支持HTML5 CANVAS";}
	}; 


	//拍照主函数
	function takePhotos(){
			//这段代 主要是获取摄像头的视频流并显示在Video 签中
			var canvas = document.getElementById("canvas"),
			context = canvas.getContext("2d"),
			video = document.getElementById("video");

			function successCallback(stream) {
				// Set the source of the video element with the stream from the camera
				if (video.mozSrcObject !== undefined) {
					video.mozSrcObject = stream;
				} else {
					video.src = (window.URL && window.URL.createObjectURL(stream)) || stream;
				}
				video.play();
			}
		
			function errorCallback(error) {
				 alert('错误代码: [CODE ' + error.code + ']');
				// Display a friendly "sorry" message to the user
			}
		 
			navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia;
			window.URL = window.URL || window.webkitURL || window.mozURL || window.msURL;
		 
			// Call the getUserMedia method with our callback functions
			if (navigator.getUserMedia) {
				navigator.getUserMedia({video: true}, successCallback, errorCallback);
			} else {
				alert('浏览器不支持getUserMedia.');
				// Display a friendly "sorry" message to the user
			}


			//这个是拍照按钮的事件，  
			document.getElementById("snap").onclick = function(){
			context.drawImage(video, 0, 0, 320, 320);     
			//获取浏览器页面的画布对象
			var canvans = document.getElementById("canvas");
			//以下开始编 数据   
			var imgData = canvans.toDataURL(); 
			//将图像转换为base64数据
			var base64Data = imgData.substr(22); 
			//将获得的base64数据设置为photo的背景图
			document.getElementById("photo").style.backgroundImage="url(data:image/png;base64,"+base64Data+")";
			document.getElementById("text").innerHTML=base64Data;


		  }
	}
```
