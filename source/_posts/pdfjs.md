---
title: Pdfjs开发说明
date: 2018-02-26 16:14:14
tags: [javascript , PDF , Mozilla ]
categories: 插件
---

PDF.js 是基于开放的 HTML5 及 JavaScript 技术实现的开源产品。pdf.js 是一个主要用于HTML5 平台上在线阅读PDF文档的小插件，基于JavaScript技术编写而成，无需任何本地技术支持。pdf.js是由Mozilla Labs发布的。他们的目标是创建一个通用的，基于标准的网络平台，能够解析和渲染PDF文件，并最终发布一个PDF阅读器扩展，毫无疑问 pdf.js 将被整合入 Gecko 成为 Firefox 的内嵌 PDF阅读器，但是具体整合时间表尚未确定
+ 兼容IE9+
+ 使用canvas渲染
[官方Demo](http://hgtech.oschina.io/ui/assets/pdfjs/web/viewer.html) [简单Demo](http://hgtech.oschina.io/ui/assets/pdfjs/demo.html);

## API
### 返回文件名称 getFilenameFromUrl
>#### PDFJS.getFilenameFromUrl(path)
>中文会被系列化
```
var title = PDFJS.getFilenameFromUrl(DEFAULT_URL);
//decodeURI(title) 反序列化
document.title = decodeURI(title);
```

### 返回pdf实例 getDocument
> #### PDFJS.getDocument(path) path地址
```
PDFJS.getDocument('helloworld.pdf').then(function(pdf) {
// you can now use *pdf* here
});
```
> > #### 返回总页数 pdf.numPages
```
pdf.numPages
//number
```

> > #### Promise]返回单页内容实例（页面引索） pdf.getPage(index)
```
pdf.getPage(1).then(function(page) {
// you can now use *page* here
})
```
> > > #### 返回页面内容(比例) page.getViewport(scale)
```
//内容实例
var viewport = page.getViewport(scale);
viewport.height;
//返回内容高度
viewport.width;
//返回内容宽度

```
> > >#### [Promise]页面呈现({画布,内容}) page.render(renderContext);
```
var scale = 1.5;
var viewport = page.getViewport(scale);

var canvas = document.getElementById('the-canvas');
var context = canvas.getContext('2d');
canvas.height = viewport.height;
canvas.width = viewport.width;

var renderContext = {
    canvasContext: context,
    viewport: viewport
};
page.render(renderContext);
```

### 返回页面显示 PDFJS.PDFPageView(config)
>自动生成画布
````
PDFJS.PDFPageView({
    container: container,//画布容器
    id: pageNum,//pdf页的引索
    scale: .9,//图片比例
    defaultViewport: page.getViewport(.9),//默认显示内容
    annotationLayerFactory: new PDFJS.DefaultAnnotationLayerFactory(),
    renderInteractiveForms: true,
})
```
> ####  PDFJS.PDFPageView().setPdfPage
>添加页面视图(内容实例)
```
PDFJS.PDFPageView().setPdfPage(page)
```
> #### PDFJS.PDFPageView().draw();
>绘制画布
```
//返回 Promise对象
PDFJS.PDFPageView().draw()
```