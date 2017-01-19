---
layout: post
title: "Pure SVG social icons"
description: "Animated social icons without JS or CSS"
tags: [svg, web design]
comments: true
HN: 13364291
share: true
---

Setting up your site you'll come to the point you need them: Social sharing buttons. Love them or hate them; You most likely can't do without them. Below is a small trip I took to making them as easy to implement as possible. Furthermore these buttons will be pure html/svg and will therefore work even with something like [NoScript](https://noscript.net/) running on the client-side.

There are two great resources that together pretty much make up the remainder of the post below. First is [sharingbutons.io](http://sharingbuttons.io/) that have amazing sharing buttons that make no use of any javascript whatsoever. The second is [svgontheweb.com](https://svgontheweb.com/) that touches upon all aspects of SVG images on the web. It ends with a thorough explanation of the ``<animate>`` tag.



## Sharing icons without javascript or trackers

Below is a set of icons taken from social icons that can be downloaded as standalone SVG icons. Embedded in an ``<img>`` tag the inline animations won't work but we'll get to that later.

<a href="/assets/social/facebook.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/facebook.svg"></a>
<a href="/assets/social/github.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/github.svg"></a>
<a href="/assets/social/google.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/google.svg"></a>
<a href="/assets/social/linkedin.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/linkedin.svg"></a>
<a href="/assets/social/mail.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/mail.svg"></a>
<a href="/assets/social/pinterest.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/pinterest.svg"></a>
<a href="/assets/social/reddit.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/reddit.svg"></a>
<a href="/assets/social/tumbler.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/tumbler.svg"></a>
<a href="/assets/social/twitter.svg" download><img style="float:left; margin-right: 1%;" width="10%" src="/assets/social/twitter.svg"></a>

Instead of the usual snippets various sites will want you to embed, you can also make a properly crafted GET request. Below are some examples that match the icons above (of course you should replace everything, including the square brackets, with your own content):

```html
<!-- Facebook -->
<a href="http://www.facebook.com/sharer.php?u=[Site URL (should be linked on facebook)]" target="_blank"></a>

<!-- Google+ -->
<a href="https://plus.google.com/share?url=[Site URL]" target="_blank"></a>

<!-- LinkedIn -->
<a href="http://www.linkedin.com/shareArticle?mini=true&amp;url=[Site URL]" target="_blank"></a>

<!-- Mail -->
<a href="mailto:?Subject=[Mail subject]&amp;Body=[Mail body]"></a>

<!-- Pinterest -->
<a href="javascript:void((function()%7Bvar%20e=document.createElement('script');e.setAttribute('type','text/javascript');e.setAttribute('charset','UTF-8');e.setAttribute('src','http://assets.pinterest.com/js/pinmarklet.js?r='+Math.random()*99999999);document.body.appendChild(e)%7D)());"></a>

<!-- Reddit -->
<a href="http://reddit.com/submit?url=[Site URL]&amp;title=[Site title]" target="_blank"></a>

<!-- Tumblr-->
<a href="http://www.tumblr.com/share/link?url=[Site URL]&amp;title=[Site title]" target="_blank"></a>

<!-- Twitter -->
<a href="https://twitter.com/share?url=[Site URL]&amp;text=[Site description]&amp;hashtags=[Site hashtags]" target="_blank"></a>
```

Of course now the only thing left is to wrap the icon in these ``<a>`` tags.



## Animate the buttons

It turned out animating the icons even for the paranoid needs some thorough testing. Where my initial icons by [Font Awesome](http://fontawesome.io/) were blocked due to the necessity to load external script, the new SVG icons too were blocked by the NoScript plugin. It turns out there is a choice to be made as to how the icons are to be included:

* Included using as an ``<img>``
* Included as an ``<object>``
* Included inline as an SVG image

All three options have their advantages. Although not as elegant, the including the SVG icons as an inline image allows for both SVG animations using the ``<animate>`` tag and will not be blocked by any script blocking plugins. Using the image tag will not allow the use of any SVG animations something that could be added making use of CSS animations instead. Using the ``<object>`` tag is something that should be done with great care. As the object tag allows the use of pure SVG animations one will now find them blocked by the mentioned plugins. Causing not only the animation to fail but also the underlying hyperlink to become unclickable.

Wanting to try out the options of using pure SVG animations (over CSS), as well as keeping the site available to all, I opted to use inline SVG images. The example below shows the easiest way to add the animation tag to an SVG element. In this case, the second ``<rect>`` will change its opacity as the mouse moves over the element.

```html
<rect stroke="#5b5b5b" fill="none">
</rect>

<rect fill="#5b5b5b" opacity="0" opacity="0">
  <animate fill="freeze" dur="0.25s" attributeName="opacity" 
    from="0" to="1" begin="mouseover"/>
  <animate fill="freeze" dur="0.25s" attributeName="opacity" 
    from="1" to="0" begin="mouseout"/>
</rect>
```

<svg xmlns="http://www.w3.org/2000/svg" style="margin-left: 45%" version="1.1" width="9%" viewBox="0 0 24 24" xml:space="preserve"> 
  <rect stroke="#5b5b5b" fill="none" width="24" height="24">
  </rect>

  <rect fill="#5b5b5b" width="24" height="24" opacity="0">
    <animate fill="freeze" dur="0.25s" attributeName="opacity" from="0" to="1" begin="mouseover"/>
    <animate fill="freeze" dur="0.25s" attributeName="opacity" from="1" to="0" begin="mouseout"/>
  </rect>
</svg>

One can however also trigger an animation using another element. For this to work make sure to assign the triggering element a unique id attribute. The snippet below shows the syntax for referring to another elements' event as a trigger.

```html
<rect stroke="#5b5b5b" fill="none" opacity="0">
  <animate fill="freeze" dur="0.25s" attributeName="opacity" 
    from="0" to="1" begin="firstRect.mouseover"/>
  <animate fill="freeze" dur="0.25s" attributeName="opacity" 
    from="1" to="0" begin="firstRect.mouseout"/>
</rect>

<rect id="firstRect" fill="#5b5b5b">
</rect>
```

<svg xmlns="http://www.w3.org/2000/svg" style="margin-left: 45%" version="1.1" width="9%" viewBox="0 0 24 24" xml:space="preserve"> 
  <rect stroke="#5b5b5b" fill="none" width="16" height="16" opacity="0">
    <animate fill="freeze" dur="0.25s" attributeName="opacity" from="0" to="1" begin="firstRect.mouseover"/>
    <animate fill="freeze" dur="0.25s" attributeName="opacity" from="1" to="0" begin="firstRect.mouseout"/>
  </rect>

  <rect id="firstRect" stroke-width="0.5pt" stroke="#5b5b5b" fill="#5b5b5b" x="8" y="8" width="16" height="16">
  </rect>
</svg>

This can be used on the icons to trigger' the animation not on the icon path itself but on an underlying rectangle. This reduces some annoying flickers on icons with small lines etc. 


