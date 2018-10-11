---
layout: js Blob 对象
title: Blob对象
date: 2018-06-19 10:49:41
tags: js
type: "tags"
---
Blob 对象表示一个不可变、原始数据的类文件对象。Blob 表示的不一定是JavaScript原生格式的数据。File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。

<!--more-->

#### 创建Blob
```
var blob = new Blob(dataArray:Array<any>,opt:{type:string});
```
dataArray：数组，包含了要添加到Blob对象中的数据，数据可以是任意多个ArrayBuffer，ArrayBufferView， Blob，或者 DOMString对象。
opt：对象，用于设置Blob对象的属性（如：MIME类型）

#### slice 方法
slice方法，截取Blob返回一个新的Blob对象。
```
slice([start[,end[,contentType]]])
```
start:可选，Blob下标，表示截取Blob的起始位置。如果为负值将会从数据的末尾开始截取。
end:可选，Blob下标，表示截取Blob的结束位置。如果为负值将会从数据的末尾开始截取。
contentType: 可选，给新的 Blob 赋予一个新的文档类型。这将会把它的 type 属性设为被传入的值。它的默认值是一个空的字符串。

```
var data01 = 'abcdefg';
var blobs01 = new Blob([datas]);
var blob2 = blobs01.slice(0,3)
console.log(blob2) // Blob(3){size:3,type:''}
```

#### Blob分段上传

File继承与Blob，可以利用Slice方法对大文件进行分割上传
```
<body>
  <input onChange='uploadFile(this)' type="file" id="file">
</body>
<script>
  function uploadFile(el) {
    var file = el.files[0];
    var chunkSize = 1024 * 1024;
    var totalSize = file.size;
    var ChunkQuantity = Math.ceil(totalSize / chunkSize);
    var offset = 0;

    var fileReader = new FileReader();
    fileReader.onload = function (e) {
      var xhr = new XMLHttpRequest();
      xhr.open('POST', 'https://www.xxxx.upload');
      xhr.onreadystatechange = function () {
        if (xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
          ++offset;
          if (offset === totalSize) {
            alert('上传成功')
          } else if (offset === ChunkQuantity - 1) {
            blob = file.slice(offset * chunkSize, totalSize);
            reader.readAsBinaryString(blob);
          } else {
            blob = file.slice(offset * chunkSize, (offset + 1) * chunkSize);
            reader.readAsBinaryString(blob);
          }
        } else {
          alert('上传失败')
        }
      }
      if (xhr.sendAsBinary) {
        xhr.sendAsBinary(e.target.result); // e.target.result是此次读取的分片二进制数据
      } else {
        xhr.send(e.target.result);
      }
    }
    var blob = file.slice(0, chunkSize);
    reader.readAsBinaryString(blob);
  }
</script>
```
#### Blob URL 图片预览
```
<body>
  <input type="file" onchange="uploadFile(this)" id="blob">
</body>
<script>
function uploadFile(e){
  var file = e.files[0];
  var url = URL.createObjectURL(file);
  var img = document.createElement('img');
      img.src = url;
  document.body.appendChild(img)
} 
</script>
```
在线代码：
<script async src="//jsfiddle.net/ptylll/grd6b3e1/embed/"></script>
参考地址：
https://juejin.im/post/59e35d0e6fb9a045030f1f35
https://developer.mozilla.org/zh-CN/docs/Web/API/Blob