## 只求极致

[ **M** ] arkdown + E [ **ditor** ] = **Mditor**    

Mditor 是一个简洁、易于集成、方便扩展、期望舒服的编写 markdown 的编辑器。
在[原作](https://github.com/houfeng/mditor)的基础上，减去jQuery依赖，实现本地图片上传功能！


## 浏览器端

##### 第一步:

引入 Mditor 样式文件  
```html
<link rel="stylesheet" href="../build/css/mditor.min.css" />
```

引用 Mditor 脚本文件
```html
<script src="https://unpkg.com/jquery@3.1.0/dist/jquery.min.js"></script>
<script src="../build/js/lrz.bundle.js"></script>
<script src="../build/js/mditor.min.js"></script>
```
说明：[lrz.bundle.js](https://github.com/think2011/localResizeIMG)是一款不错的前端图片处理插件。

##### 第二步:

添加 textarea 元素
```html
<textarea name="editor" id="editor">
```

创建 Mditor 实例
```javascript
var mditor = new Mditor("#editor",{
	height:300,
	fixedHeight:true,
	picture:options
});
```
说明:
* `[options]` 这个参数不允许忽略
  * `url {String}` 图片上传的后端处理地址
  * `width {Number}` 图片最大不超过的宽度，默认为原图宽度，高度不设时会适应宽度。
  * `height {Number}` 同上
  * `quality {Number}` 图片压缩质量，取值 0 - 1，默认为0.7
  * `fieldName {String}` 后端接收的字段名，默认：file

## 后端处理
nodejs + [thinkjs](https://thinkjs.org/)版本后端
```
pictureAction(){
      // 储存图片
      let file = this.file('file');
      let filepath = file.path;
      let filename = file.originalFilename;
      //去除扩展名的文件名
      let param=filename.lastIndexOf('.') >= 0
                  ? filename.slice(0,filename.lastIndexOf('.')):'';
      //文件上传后，需要将文件移动到项目其他地方，否则会在请求结束时删除掉该文件
      let uploadPath = think.RESOURCE_PATH + '/static/upload';
      think.mkdir(uploadPath);
      let basename = path.basename(filepath);
      fs.renameSync(filepath, uploadPath + '/' + basename);
      let imgurl="/static/upload/"+basename;
      this.json({imgurl});
  }
```