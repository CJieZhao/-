# GitBook 本地配置
---

## windows平台:

### 安装
1. 安装 [Node](http://www.nodejs.org) 
1. 执行 node cli.js install npm -gf
1. 执行 npm install gitbook -g
1. 如需PDF需安装
	* [ebook-convert](http://www.calibre-ebook.com/download_windows)
	* [phantomjs](http://phantomjs.org/download.html)

### HTML 版
1. 进入 book 目录, 执行 gitbook build --gitbook=2.6.7 生成HTML版(不指定2.6.7页面不能跳转).

### PDF 版
1. 进入 book 目录, 执行 gitbook pdf
