<script src="raphael-min.js"></script>
this a test,H1
==
这是一个md文件测试,H2
---
# 这是H1
## 这是H2
####### 这是H6

区块引用 Blockquotes
>This is a blockquote with 2 paragraphs.Lorem ipsum dolor sit amet
>consectetuer adipiscing elit.
>hahahahahaheeehehehehuoohhhlhjlkjkjemememeda
>ssss

>Donec sit amet nisl.
>wjlkjlkjlk
>##h2

>1. asd  
>2.fasfw

* red  
* Green  
* Blue

+ Red
+ Green
+ Blue

- Red
- Green
- Blue

1. Bird
2. McHale
3. Parish

* Lorem inalsnlj
  adwawewerawr
* jwkqljeqwhuohaljkj donawnklje

* bird
* magic

* A list item with a blockquote:
  >This is a blockquote
  >inside a list item
  
* 一列表项包含一个列表区块：
	<代码挖掘了空间了空间啊三等了for
	for item in items:
		x = y
	>  
123\. 这是数字123

这是一个普通段落：  
	这是一个代码区块。

Here is an example of AppleScript:

	tell application "Foo"
		beep
	end tell  
***********
-------

#链接
行内式
This is [an expample](https://www.baidu.com/ 'Title') inline link
[This link](https://www.baidu.com)has no title attribute.

This is

参考式：
This is [an example][id] reference-style link

[id]:http://www.bilibili.tv/ 'hahaha'

	'''flow
	st=>start: Start|past:>http://www.google.com[blank]
	e=>end: End:>http://www.google.com
![](http://i.imgur.com/JgKjTJx.jpg)	op1=>operation: My Operation|past
	op2=>operation: Stuff|current
	sub1=>subroutine: My Subroutine|invalid
	cond=>condition: Yes 
	or No?|approved:>http://www.baidu.com
	c2=>condition: Good idea|rejected
	io=>inputoutput: catch something...|request
	
	st->op1(right)->cond
	cond(yes, right)->c2
	cond(no)->sub1(left)->op1
	c2(yes)->io->e
	c2(no)->op2->e
	'''
```flow
st=>start: 开始1
e=>end: 结束1
op=>operation: 我的操作2
cond=>condition: 确认？
st->op->cond
cond(yes)->e
cond(no)->op
```
