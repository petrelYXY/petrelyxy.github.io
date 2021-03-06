---
layout: post
title:  "RG阅读笔记!"
date:   2017-02-07 11:25:36 +0800
desc: "record how i read RG."
keywords: "RG"
categories: [Mess]
tags: [RG]
icon: icon-html
---
#说明
RG项目使用Django框架来做开发，Django框架使用的为MTV模式（Models，Templates，Views），与MVC模式类似  
* **Model** ：负责业务对象和数据库的关系映射（ORM） 。   
* **Template** ： 负责如何把页面展示给用户（html）。    
* **View** ： 负责业务逻辑。  
***需要一个URL分发器***，将URL的页面请求分发给不同的View处理。在RG项目中，它位于project/urls.py    

值得注意的是，使用的数据库为SSDB，而Django并没有能与SSDB良好交互的api，所以此项目中并没有定义数据对象的Model，而是在/ResearchGo/BaseSSDB.py中封装了SSDB数据库的CRUD等操作。所以Django自带的后台管理功能也许无法良好的利用。  本文主要关注与View。  

在已经写好的项目文件中，view文件位于'ResearchGo/'而不是'ResearchGo/view/'下。  

[/ResearchGo](#RG)下主要负责数据库操作封装，业务逻辑和用户交互。  
[/server](#server)下主要负责处理爬取好的数据，并将它们更新到数据库中。

## [页面跳转：urls.py](http://python.usyiyi.cn/translate/django_182/ref/urls.html#url)  
### patterns()
主要使用了patterns()函数（自django1.8版本已被启用），urlpatterns应该是django.conf.urls.url()实例的简单列表。  
一个函数，它接受前缀和任意数量的URL模式，并返回django需要的格式的URL模式列表。  

patterns()的第一个参数是字符串prefix。示例如下：  
	from django.conf.urls import patterns, url
	urlpatterns = patterns('',  
		url(r'^articles/([0-9]{4})/$', 'news.views.year_archive'),
		url(r'^articles/([0-9]{4})/([0-9]{2})/$', 'news.views.month_archive'),
		url(r'^articles/([0-9]{4})/(0-9){2})/([0-9]+)$', 'news.views.article_detail'),
)

在此示例中， 每个示例都有一个**公共前缀**-‘news.views’。可以使用patterns()函数的第一个参数来指定要应用于每个视图函数的前缀， 而不是在urlpatterns中为每个条目输入该值。  
则上面的例子可以更简洁地写成：  
	from django.conf.urls import patterns, url
	urlpatterns = patterns('news.views',  
		url(r'^articles/([0-9]{4})/$', 'year_archive'),  
		url(r'^articles/([0-9]{4})/([0-9]{2})/$', 'month_archive'),  
		url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', 'article_detail),  
)

patterns()是一个函数调用，它最多接受255参数。意识到patterns()返回一个Python列表，所以可以拆分列表的构造。
	urlpatterns = patterns('',  
	...
	)  
	urlpatterns += patterns('',  
	...
	)  
### url()  
url(regex, view, kwargs = None, name = None, prefix =")[source]  
urlpatterns应该是url()实例的列表。例如  
urlpatterns = [  
	url(r'^index/$', index_view, name = "main-view"),  
]

<span id = 'RG'></span>
## /ResearchGo  
/ResearchGo下的文件主要的作用是业务逻辑和与用户交互。  

<span id = 'server'></span>
## /server  
对于每一个需要处理的目标(期刊/会议)都具有两个处理的文件baseinfo和cited，如下面的路径所示：  
	yangxinyu@yangxinyu-QJW4:/media/yangxinyu/WINPE_YXY/Data/New_Data/SIMULATION MODELLING PRACTICE AND THEORY2003-2003$ ls  
	SIMULATION MODELLING PRACTICE AND THEORY2003-2003Cited.json  
	SIMULATION MODELLING PRACTICE AND THEORY2003-2003.json  
在使用Insert.py插入paper表时会使用Journal基本信息表中的信息，所以**若Journal基本信息表为空，应当先执行对Journal基本信息表的插入**


baseinfo中存储着期刊中每个文章的具体相关信息，cited中存放着文章的被引用信息。从这两个文件获取需要的信息并插入数据库中，首先要了解文件的结构。在传递来的json文件中，这一个文件被当作一个大的列表传递进来，每一个列表项为一个字典，字典中即是文章的具体相关信息。  

### Insert.py
从baseinfo和cited两个json文件中提取相关的信息并插入数据库表中。其中Paper表中存有文章的详细相关信息，而Journal表、Author表、Keyword表和organization表用来映射paper表，拥有相似的结构和统一的value格式。  
Class Insert():  
|--__init__(self,ssdb,filename,basedata,citedata,paperId=None)  
|--__call__(self)  
|--__del__(self)  
|--getData(self):  利用PaperItems(一个以字典为列表项的列表，字典包含一个文章的相关信息)来根据文章标题来匹配两个文件中的文章，然后将cited中的信息添加到PaperItems中，通过比较PaperItems和CiteItems中的信息完成了对两个json文件的遍历，**并将文章的cited信息添加到了PaperItems中**，这也是此函数的目标功能。接下来通过insertData完成数据的插入  
|--insertData(self,PaperItem):对PaperItems的一个项进行去空格、大小写处理、年份段判断、格式比较转换、计算文章档次、过长信息截短等操作后将数据插入相关数据表中。其中有paper表、journal表、organization表、email表、author表、author_baseinfo表、keyword表。  
|--paper(self,name,dic):插入一个字典到$Paper$+name中，在insertData中被调用来插入文章信息。  
|--paper_one(self,name,key,value):将key-value插入到$Paper$+name表中，用于对hashmap插入单个键值，如插入计算后的level值。  
|--author(self,name,key,value):插入author表  
|--author_baseinfo(self,key,value):插入author_baseinfo表  
|--paperemail(self,name,key,value):插入PaperEmial表  
|--keyword(self,name,key,value):插入keyword表  
|--journal(self,name,key,value):插入journal表  
|--organization(self,name,key,value):插入organization表  
|--categories_cite(self,Journal_ls):计算被引用的情况  
|--transform(self,PaperItem):调用json.dumps对数据进行格式转化  
