---
layout: post
title: Jekylk markdown入门
description: "markdown语法入门学习，留给自己写blog用"
modified: 2014-07-13
category: articles
tags: [markdown,语法,jekyll]
image:
  feature: so-simple-sample-image-1.jpg
  credit: william
comments: true
share: true
---

##特别注意 
特别需要注意，部分markdown标签，需要在上面一行进行空行，否则无法识别。

## 1. 几种常用的表达方式

###6级标题

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

### Body text

### 几种字体
**This is strong**   粗体字
*This is emphasized* 斜体 强调字体
H<sub>2</sub>O  小字体  水
<cite>(That’s a citation)</cite>  引用
<u>Underline</u>  下划线
<abbr title="cascading stylesheets">CSS<abbr>  注释，当鼠标停止在上面的时候会进行显示。

### 链接插入
`[这儿跳转到我的主页](http://pengjunjie.com/)`
[这儿跳转到我的主页](http://pengjunjie.com/)

### 图片嵌入
![Smithsonian Image]({{ site.url }}/images/3953273590_704e3899d5_m.jpg)
{: .pull-right}
图片在右边嵌入显示

### 横块引用
> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

### 列表模式
列表模式，其中还包含子列表
1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

一般列表
* Item one
* Item two
* Item three

### 表格，这个是在太麻烦了

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}


### 按钮button
<div markdown="0"><a href="#" class="btn">This is a button</a></div>
`<div markdown="0"><a href="#" class="btn">This is a button</a></div>`
这是一个按钮
按钮只需要使用class=btn就可以了，应该是这么定义的。

##2 图片的插入

<figure>
     <a href="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_b.jpg"><img src="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_c.jpg"></a>
     <figcaption><a href="http://www.flickr.com/photos/80901381@N04/7758832526/" title="Morning Fog Emerging From Trees by A Guy Taking Pictures, on Flickr">Morning Fog Emerging From Trees by A Guy Taking Pictures, on Flickr</a>.</figcaption>
</figure>

<figure class="half">
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <img src="http://placehold.it/600x300.jpg">
     <img src="http://placehold.it/600x300.jpg">
     <figcaption>Two images.</figcaption>
</figure>

<figure class="third">
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
     <figcaption>Three images.</figcaption>
</figure>

类似这种的插入有两种方式，直接使用img标签在外层就会直接显示为图片，也不带超链接。如果有超链接方式就会在外面包一层a标签，这样的画图片初始状态为略带灰色的状态，当鼠标点击上去的时候会显示出完全状态。
后面的figcaption就是代表图片的说明，同样可以在里面做超链接等等。

在一个figure中的图片会根据图片的多少和大小来自动的排列。

##3 在顶部嵌入图片
{% raw %}
---
layout: post
title: "Post with Large Feature Image and Text"
description: "Custom written post descriptions are the way to go... if you're not lazy."
category: articles
tags: [sample post, readability]
modified: 2013-06-30
image:
  feature: so-simple-sample-image-3.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
comments: true
share: true
—--
{% endraw %}

这个嵌入做的是在文档的开头上部嵌入一个图片，来做类似一个banner的效果。 可以看到也可以添加自己的credit 以及相应的图片链接。

##5 嵌入视频

{% highlight html %}
<iframe width="560" height="315" src="http://www.youtube.com/embed/SqYiglufb8Y" frameborder="0"> </iframe>
{% endhighlight %}
这样就可以嵌入视频了，不知道国内的优酷可以不可以，这样倒是可以嵌入google地图的，后面有时间看看能不能嵌入一个优酷视频

##6 页面标题链接


对于页面标题链接可以直接使用在头部使用 link标签来进行超链接，如下所示。

{% raw %}
---
layout: post
title: "Sample Link Post"
description: "Example and code for using link posts."
category: articles
tags: [sample post, link post]
comments: true
link: http://mademistakes.com 
---
{% endraw %}

他会自动为链接做一个剪头指示超链接，挺有意思的。

##7 代码显示


###7.1 源码显示

注脚：
在文中引用[^1]
在问后申明，作为引用注脚的作用 [^1]: <http://en.wikipedia.org/wiki/Syntax_highlighting>

单行代码使用 `/assets/less/pygments.less`

###7.2 Pygments Code blocks

Pygments代码风格使用文件/assets/less/pygments.less，通过main.less来进行编译

CSS风格

{% highlight css %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}


HTML风格

{% highlight html %}
{% raw %}
<nav class="pagination" role="navigation">
    {% if page.previous %}
        <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
    {% endif %}
    {% if page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
    {% endif %}
</nav><!-- /.pagination -->
{% endraw %}
{% endhighlight %}

这其中的{% raw %}就是显示嵌入滚动条的意思。

ruby风格

{% highlight ruby %}
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
{% endhighlight %}


如果想深入学习可以参考 [pygments主页](http://pygments.org)
一种将源码更漂亮的表达的方式

###7.3 Standart Code Block

{% raw %}
    <nav class="pagination" role="navigation">
        {% if page.previous %}
            <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
        {% endif %}
        {% if page.next %}
            <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
        {% endif %}
    </nav><!-- /.pagination -->
{% endraw %}

标准的代码块模式直接放在`{% raw %}` 中就可以了。

###7.4 Fenced Code Blocks
另一种代码块模式

位于 /assets/less/coderay.less 处并在main.less 处编译和倒入
同样在_config.yml 也可以对coderay进行相关的配置

这种配置方法也比较有意思
####css类型使用如下的表示方法

~~~ css
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
~~~


####html如下：

~~~ html
{% raw %}<nav class="pagination" role="navigation">
    {% if page.previous %}
        <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
    {% endif %}
    {% if page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
    {% endif %}
</nav><!-- /.pagination -->{% endraw %}
~~~

####ruby如下

~~~ ruby
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
~~~

详细的参考可以查看这里[Fenced Code Blocks](https://pythonhosted.org/Markdown/extensions/fenced_code_blocks.html)



