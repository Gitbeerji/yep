### JAVASCRIPT压缩图片

用户本地的图片体积都很大，尺寸不变的绘制会失败

如果要保证尺寸不变的上传，一般就直接FileReader读取文件然后塞到formdata中，不压缩上传文件，还想前端压缩，只能尝试把大图片分解成多个瓦片来逐个上传，后端在拼接实现。

如果能接受修改绘制尺寸，把大图绘制到一个等比大小的canvas上，然后读取这歌小canvas的base64上传。

另外，上传base64字符串其实不是一个很好的方案，base64的体积很大。

twitter图片上传逻辑

```javascript

function converCanvasToBlob(canvas) {
	var format = "image/jpeg";
	var base64 = canvas.toDataURL(format);
	var code = window.atob(base64.split(",")[1]);
	var aBuffer = new window.ArrayBuffer(code.length);
	var uBuffer = new window.Uint8Array(aBuffer);	
	for(var i = 0;i < code.length; i++){
		uBuffer[i] = code.charCodeAt(i);
	}
	var Builder = window.WebkitBlobBuilder || window.MozBlobBuilder;

	if(Builder){
		var builder = new Builder;
		builder.append(buffer);
		return builder.getBlob(format);
	}else {
		return new window.Blob([buffer],{type: format});
	}
}

```

这是触屏版上传前的图片压缩逻辑之一，就是在前端把base64转成二进制数据，这个数据体积相比base64小很多，还可以塞到formdata中提交，不过不支持android2以下，ios5.1以下版本的浏览器。


由于IOS的限制，所以思路是将图片分解成若干块，一块一块来进行绘制。最后拼在一张画布上，则不会出现出现不能绘制的情况。其次，在绘制的过程中，每次绘制一块，绘制大小也不能超过2M，否则canvas一样会被挂起。

国外的项目可以参考[stomita/ios-imagefile-megapixel · GitHub](https://github.com/stomita/ios-imagefile-megapixel)