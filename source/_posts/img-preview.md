---
title: 图片预览的几种方法
---

方法有三种：

```
1. 选择文件后直上传然后得到网络url
2. 用HTML5的File API的FileReader图片本地转成base64格式的url
3. 用URL.createObjectURL(file)对象方法创建临时路径
```
<!--more-->

## 选择文件后直接上传然后得到网络url
利用2、3方法先将图片传到服务器，再使用服务器里面的绝对地址

## 用HTML5的File API的FileReader获取图片的base64字符串
``` javascript
  // 下面是使用vue的写法

  imgPreview (file) {
      let self = this;
      // 看支持不支持FileReader
      if (!file || !window.FileReader) return;

      if (/^image/.test(file.type)) {
          // 创建一个reader
          let reader = new FileReader();
          // 将图片将转成 base64 格式
          reader.readAsDataURL(file);
          // 读取成功后的回调
          reader.onloadend = function () {
              // 将图片展示
              self.dataUrl = reader.result;
          }
      }
  },
  handleFileChange (e) {
      ...
      this.file = inputDOM.files[0];
      ...
      // 在获取到文件对象进行预览就行了！
      this.imgPreview(this.file);
      ...
  }
```


## URL 对象方法创建临时路径


``` javascript
imgBlob = imgInput.files[0] // 获取blob对象

var srcTemp = URL.createObjectURL(imgBlob) // 创建临时路径

URL.revokeObjectURL(srcTemp) // 销毁临时路径
```
