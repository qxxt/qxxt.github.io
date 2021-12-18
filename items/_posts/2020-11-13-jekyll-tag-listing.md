---
layout: post
title: Tag listing in Jekyll with CSS
tags: [css, jekyll, liquid]
category: "web"
date: 13 Nov 2020
---

As far as I know, it isn’t possible to pass query parameter to Jekyll. Which is quite obvious considering they are “just” *a static page generator*(??). Due to that limitation it isn’t possible to lists a posts and `collections.items` with a specific tag without creating it's page individually. Though, you can use plugin or create your own plugin with Ruby. But still, fuck it.

While it isn’t possible to pass a query to Jekyll. You can still use an `id` (that “#” at the end of the url). We will use that to hid and show posts with CSS post depending on the current ids.

First thing first, we’ll need to create an element with tag name as its id on top of all elements or/ and parent elements that will be triggered to hide/ show. We’ll use this as a *trigger*.
```html
{% raw %}<span id="{{ site.posts | map: 'tags' | uniq | join: '"></span><span id="' }}"></span>{% endraw %}
```

And then CSS. 
```css
{% raw %}// hide all post by default
.post {
    display: none;
}
// show posts with "tags" attribute matched the ids
{% for tag in site.tags %}#{{tag[0]}}:target ~ [tags~={{tag[0]}}]{% if forloop.last == false %}, {% endif %}{% endfor %} {
    display: block;
}{% endraw %}
```
Notice the syntax is messed up around bracket and stuff. They're called [Liquid](https://shopify.github.io/liquid/){: target="_blank"}. I can't really explain it and <span class="spoiler">you don't really need to know much about it either</span>.

Next, listting all the post. We'll add a new attribute called "tags" and use the post's associated tags as its value. 
```html
{% raw %}{% for post in site.posts %}
    <article class="post" tags="{% for tag in post.tags %}{{tag}}{% if forloop.last == false %}{{" "}}{% endif %}{% endfor %}">
        <h3><a href="{{post.url}}">{{post.title}}</a></h3>
    </article>
{% endfor %}{% endraw %}
```
Now if you go to `pageurl#tag` it will show all the post with a tag "tag". You can see the demo here [/unlisted/tags.html#jekyll](/unlisted/tags.html#jekyll){: target="_blank"}. In this url, it will show all the post (in this blog) that has a tag: "jekyll" in it.

The whole code will look like this:
```html
{% raw %}---
---
<span style="display:none;" id="{{ site.posts | map: 'tags' | uniq | join: '"></span><span id="' }}"></span>
<style>
    .post {
        display: none;
    }
    {% for tag in site.tags %}#{{tag[0]}}:target ~ [tags~={{tag[0]}}]{% if forloop.last == false %}, {% endif %}{% endfor %} {
        display: block;
    }
</style>
{% for post in site.posts %}
    <article class="post" tags="{% for tag in post.tags %}{{tag}}{% if forloop.last == false %}{{" "}}{% endif %}{% endfor %}">
        <h3><a href="{{post.url}}">{{post.title}}</a></h3>
    </article>
{% endfor %}{% endraw %}
```
My [tags.html](/tags.html){: target="_blank"} page is an example where this is applied. You can see the source [here](https://github.com/rxxb/rxxb.github.io/blob/main/tags.html){: target="_blank"}.
