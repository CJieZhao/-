# GitBook 本地配置
---

## windows平台:

### 安装
1. 安装 [Node](http://www.nodejs.org)
1. 解压 npm-4.1.1.zip, 并进入目录.
1. 执行 node cli.js install npm -gf
1. 执行 npm install -g gitbook-cli
1. 如需PDF需安装
	* [ebook-convert](http://www.calibre-ebook.com/download_windows)
	* [phantomjs](http://phantomjs.org/download.html) (未发现用处)

### HTML 版
1. 进入 book 目录, 执行 gitbook build \-\-gitbook=2.6.7 <BR>生成HTML版(不指定2.6.7页面不能跳转).

### PDF 版
1. 进入 book 目录, 执行 gitbook pdf
