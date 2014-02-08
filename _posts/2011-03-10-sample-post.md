---
layout: post
title: Sample Post
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2013-05-31
category: blog
tags: [sample post]
image:
  feature: so-simple-sample-image-1.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
comments: true
share: true
---

Below is just about everything you'll need to style in the theme. Check the source code to see the many embedded elements within paragraphs.

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

### Body text

Lorem ipsum dolor sit amet, test link adipiscing elit. **This is strong**. Nullam dignissim convallis est. Quisque aliquam.

![Smithsonian Image]({{ site.url }}/assets/img/3953273590_704e3899d5_m.jpg)
{: .pull-right}

*This is emphasized*. Donec faucibus. Nunc iaculis suscipit dui. 53 = 125. Water is H<sub>2</sub>O. Nam sit amet sem. Aliquam libero nisi, imperdiet at, tincidunt nec, gravida vehicula, nisl. The New York Times <cite>(That’s a citation)</cite>. <u>Underline</u>. Maecenas ornare tortor. Donec sed tellus eget sapien fringilla nonummy. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus.

HTML and <abbr title="cascading stylesheets">CSS<abbr> are our tools. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus. Praesent mattis, massa quis luctus fermentum, turpis mi volutpat justo, eu volutpat enim diam eget metus.

### Blockquotes

> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

## List Types

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### Unordered Lists

* Item one
* Item two
* Item three

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

<div markdown="0"><a href="#" class="btn">This is a button</a></div>


Here are some examples of what a post with images might look like. If you want to display two or three images next to each other responsively use `figure` with the appropriate `class`. Each instance of `figure` is auto-numbered and displayed in the caption.

## Figures (for images or video)

### One Up

<figure>
	<a href="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_b.jpg"><img src="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_c.jpg"></a>
	<figcaption><a href="http://www.flickr.com/photos/80901381@N04/7758832526/" title="Morning Fog Emerging From Trees by A Guy Taking Pictures, on Flickr">Morning Fog Emerging From Trees by A Guy Taking Pictures, on Flickr</a>.</figcaption>
</figure>

### Two Up

Apply the `half` class like so to display two images side by side that share the same caption.

{% highlight html %}
<figure class="half">
	<img src="/assets/img/image-filename-1.jpg">
	<img src="/assets/img/image-filename-2.jpg">
	<figcaption>Caption describing these two images.</figcaption>
</figure>
{% endhighlight %}

And you'll get something that looks like this:

<figure class="half">
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<img src="http://placehold.it/600x300.jpg">
	<img src="http://placehold.it/600x300.jpg">
	<figcaption>Two images.</figcaption>
</figure>

### Three Up

Apply the `third` class like so to display three images side by side that share the same caption.

{% highlight html %}
<figure class="third">
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<figcaption>Caption describing these three images.</figcaption>
</figure>
{% endhighlight %}

And you'll get something that looks like this:

<figure class="third">
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<a href="http://placehold.it/1200x600.jpg"><img src="http://placehold.it/600x300.jpg"></a>
	<figcaption>Three images.</figcaption>
</figure>