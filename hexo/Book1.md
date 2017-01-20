# Hexo
---
## 安装和使用
1. 安装 Node.js.
1. 安装 Hexo (npm \-install hexo-cli)
1. 本地创建工作目录

	* hexo init <folder>
	* cd <folder>
	* npm install
	* hexo generate

1. 本地查看效果

	+ cd <folder>
	+ npm install
	+ hexo server


## Markdown 文件头格式:
1. md文件头 

	```
	title: #文章标题
	date: #文章日期
	tags: #文章标签
	categories: #文章分类
	---
	```

## 使用百度统计  

1. 在主题的 layer/_partial 下创建 baidu_tongji.ejs, 并写入内容如下:

	```
	<% if (theme.baidu_tongji){ %>
	<!-- baidu_tongji -->
	<script type="text/javascript">
		var _hmt = _hmt || [];
		(function() {
		  var hm = document.createElement("script");
		  hm.src = "https://hm.baidu.com/hm.js?a8319d4e3f080cced1791a7aa04b658d";
		  var s = document.getElementsByTagName("script")[0]; 
		  s.parentNode.insertBefore(hm, s);
		})();
	</script>
	<!-- End baidu_tongji -->
	<% } %> 
	```

2. 在主题的 layer/_partial/head.ejs 添加如下代码:
	
	```
	<%- partial('baidu_tongji') %>
	```
