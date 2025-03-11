---
layout: post
title: 'kramdown语法'
categories: test kramdown
description: some word here
keywords: markdown
date: 2017-06-02 14:14:52 +0800
---

> 原文地址：<https://kramdown.gettalong.org/syntax.html>

### blank line

一个或多个blank line 会被当做一个blank line 来处理

### \<br/\>

在一行文本末尾试用 \\\\ 或 `tab` 或 两个空格，可实现回车换行，例如：   

这是一个行文字，并在末尾使用上面的方式回车换行    
现在换行了。否则，   
这是一个行文字，末使用上面的方式回车换行
现在被连接到上一行末尾。

### Paragraphs

{{site.indent}}这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落
这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落
这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落    
{{site.indent}}这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落
这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落
这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落这是一个段落 

### headers
kramdown 支持Setext style和atx style两种header。

#### Setext style
    
    First level header
    ==================
    
    Second level header
    ------
    

#### atx style
    
    # First level header
    
    ### Third level header    ###
    
    ## Second level header ######
    
### Blockquotes

> ## Headers work
> * lists too
> This is a blockquote.
> > on multiple lines     
> > > that may be lazy.
> > > > This is the second paragraph.   
laskdjflkjf
> * lists too
> >lsadkfjlkjsfdlk

> A code block:
>
>     ruby -e 'puts :works'

### Code Blocks
#### Standard Code Blocks
四个空格或一个tab，然后跟被code block的文本

    Here comes some code    
    This text belongs to the same code block.
    
如果在一个code block后紧跟另一个

        Here comes some code
    ^
        This one is separate.
        
效果如下:

^
    Here comes some code
^
    This one is separate.
    
#### Fenced Code Blocks

~~~~
~~~
code with tildes
~~~
~~~~

#### Language of Code Blocks

~~~~
~~~
def what?
  42
end
~~~
{: .language-ruby}

or

~~~ ruby
def what?
  42
end
~~~

~~~~

效果如下：

~~~
def what?
  42
end
~~~
{: .language-ruby}

~~~ ruby
def what?
  42
end
~~~

### lists

#### ordered and unordered lists

~~~
* kram
+ down
- now

1. kram
2. down
3. now
~~~

#### Definition Lists

~~~
kramdown
: A Markdown-superset converter

Maruku
: Another Markdown-superset converter
~~~

### Tables
~~~
|-----------------+------------+-----------------+----------------|
| Default aligned |Left aligned| Center aligned  | Right aligned  |
|-----------------|:-----------|:---------------:|---------------:|
| First body part |Second cell | Third cell      | fourth cell    |
| Second line     |foo         | **strong**      | baz            |
| Third line      |quux        | baz             | bar            |
|-----------------+------------+-----------------+----------------|
| Second body     |            |                 |                |
| 2 line          |            |                 |                |
|=================+============+=================+================|
| Footer row      |            |                 |                |
|-----------------+------------+-----------------+----------------|
~~~
^
~~~
|---
| Default aligned | Left aligned | Center aligned | Right aligned
|-|:-|:-:|-:
| First body part | Second cell | Third cell | fourth cell
| Second line |foo | **strong** | baz
| Third line |quux | baz | bar
|---
| Second body
| 2 line
|===
| Footer row
~~~

效果如下：

|-----------------+------------+-----------------+----------------|
| Default aligned |Left aligned| Center aligned  | Right aligned  |
|-----------------|:-----------|:---------------:|---------------:|
| First body part |Second cell | Third cell      | fourth cell    |
| Second line     |foo         | **strong**      | baz            |
| Third line      |quux        | baz             | bar            |
|-----------------+------------+-----------------+----------------|
| Second body     |            |                 |                |
| 2 line          |            |                 |                |
|=================+============+=================+================|
| Footer row      |            |                 |                |
|-----------------+------------+-----------------+----------------|

### Horizontal Rules

~~~
* * *

---

  _  _  _  _

---------------
~~~
效果如下：
* * *

### Math Blocks

### HTML Blocks

### Links and Images
#### Automatic Links
~~~
url: <https://esonger.github.io>
email: <zhaoqiang_zq@126.com>
~~~
效果如下：  
url: <https://esonger.github.io>  
email: <zhaoqiang_zq@126.com>

#### Inline Links

This is [a link](http://rubyforge.org) to a page.
A [link](../test "local URI") can also have a title.
And [spaces](/index.html)!

#### Reference Links

#### Link Definitions

### Images


~~~
Here comes a ![smiley](../images/smiley.png)! And here
![too](../images/other.png 'Title text'). Or ![here].
With empty alt text ![](see.jpg)
~~~