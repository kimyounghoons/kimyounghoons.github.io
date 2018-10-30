---
layout: post
title: Android Studio
description: "first test"
modified: 2018-10-11
tags: [android]
categories: [android]
---
Android Blog Test
**This is strong**. 

![Smithsonian Image]({{ site.url }}/images/3953273590_704e3899d5_m.jpg)
{: .image-right}

*This is emphasized*. 

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

Unordered Lists
<!-- 
* Item one
* Item two
* Item three -->

## Tables

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

## Code Snippets

Syntax highlighting via Pygments

{% highlight css %}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
{% endhighlight %}

Non Pygments code example

    <div id="awesome">
        <p>This is great isn't it?</p>
    </div>

## Buttons

Make any link standout more when applying the `.btn` class.

{% highlight html %}
<a href="#" class="btn btn-success">Success Button</a>
{% endhighlight %}

<div markdown="0"><a href="#" class="btn">Primary Button</a></div>
<div markdown="0"><a href="#" class="btn btn-success">Success Button</a></div>
<div markdown="0"><a href="#" class="btn btn-warning">Warning Button</a></div>
<div markdown="0"><a href="#" class="btn btn-danger">Danger Button</a></div>
<div markdown="0"><a href="#" class="btn btn-info">Info Button</a></div>

## Post Featured Image

To show an image banner at top of your post, use add the following code:

{% highlight yaml %}
image:
    feature: <name of the image>
    credit: <source of the image - optional>
    creditlink: <source of the image (url) - optional>
{% endhighlight %}

The image must be in the ``/images`` folder.
