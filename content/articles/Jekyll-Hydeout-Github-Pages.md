+++
title = "Jekyll + Hydeout + Github Pages"
date = "2018-10-15T14:10:29Z"
description = ""
slug = ""
tags = ["Jekyll"]
+++

I recently migrated this website from [Wordpress](http://wordpress.com) to [Github pages](http://pages.github.com/)
using [Jekyll](http://jekyllrb.com) and [Hydeout](https://github.com/fongandrew/hydeout). Over time, I grew to really dislike how heavy-weight Wordpress is. The WYSIWYG editors make it really hard to see what HTML is being generated and tend to bloat the code that is produced. I found the Wordpress plugin system so confusing that I was afraid to try to customize the layout of anything.

The process of backing up Wordpress is a huge pain. I was annoyed constantly installing security updates. I hated having to keep an opaque database in order to keep all of my content. And whenever I went in to edit my website, I was always afraid that I would fat-finger change something and have no idea what happened.

So when I learned about [Jekyll](http://jekyllrb.com/), it seemed like a great alternative. I like the idea that my entire blog is a set of static files. Besides its simplicity, it makes backups so much easier and avoids most common security concerns caused by running dynamic websites. I could write my posts in [Markdown](http://en.wikipedia.org/wiki/Markdown), which is quite handy. Also, Jekyll allows for code examples to be very nicely embedded in the website. Finally, Jekyll is very lightweight and allows for very minimal websites without any bloat.

The fact that GitHub provides [free hosting for Jekyll blogs](http://pages.github.com) is just icing on the cake. It will save me a few extra dollars each year in hosting. GitHub provides automatic version control of my blog. I can use GitHub's web editor to write blog posts online. And I can still connect it to my custom domain [azharkhan.in](https://azharkhan.in).

## Getting Started

I came across this really cool git repo called [Hydeout](https://github.com/fongandrew/hydeout) which was super easy to setup. Hydeout is a theme built on top of [poole](https://github.com/poole/poole). Hydeout updates the original [Hyde](https://github.com/poole/hyde) theme of poole for Jekyll 3.x and adds new functionality.

[fongandrew.github.io](https://fongandrew.github.io/hydeout/) is a working demo of the Hydeout website that looks like this

![The demo Hydeout website](/assets/demo.png)

To get started with my blog, all I had to do was create a new Git repo with the name [azkh93.github.io](http://azkh93.github.io), download the Hydeout repository, and push it to my git repo. A few minutes later the website azkh93.github.io was ready! I only had a few posts on my previous website so I just copied them over manually. But there is [a package](http://jekyllrb.com/docs/migrations) for migrating blogs to Jekyll.

## Blog Layout

The initial state of the Hydeout repository is:

{% highlight sh %}
$ ls -1
404.html
Gemfile
CNAME
LICENSE.md
README.md
_config.yml
_includes
_layouts
_posts
_sass
_screenshots
assests/css
category
about.md
favicon.ico
favicon.png
atom.xml
index.html
search.html
tags.html
{% endhighlight %}

You can view the folder structure on [GitHub](https://github.com/fongandrew/hydeout).
When you run Jekyll, it creates a folder called _site with the
static website inside of it. Every file or folder in the repo will get copied
into the _site folder unless it begins with an underscore.
Markdown files will get automatically converted to HTML
and Hydeout uses the [Liquid](http://liquidmarkup.org) templating system to allow
for somewhat dynamic content on the website.

The folder [_posts](https://github.com/fongandrew/hydeout/tree/master/_posts) contains all of the blog posts in markdown format.
Some example posts that come with Hydeout are:
{% highlight bash %}
$ ls -1 _posts/
2013-12-31-whats-jekyll.md
2014-01-01-example-content.md
2014-01-02-introducing-hyde.md
{% endhighlight %}
[index.html](https://github.com/fongandrew/hydeout/blob/master/index.html)
contains the front page of the blog and
[about.md](https://github.com/fongandrew/hydeout/blob/master/about.md) is a
static post in markdown format.
If you want to have more static files, you can just add them to the
repo and Hydeout will copy them to the _site folder when rendering the website.




[_config.yml](https://github.com/fongandrew/hydeout/blob/master/_config.yml)
contains general configuration stuff for the website:
{% highlight yaml %}
## Setup
title:            Hydeout
tagline:          'The Jekyll theme'
description:      'The <a href="http://hyde.getpoole.com" target="_blank">Hyde</a> theme for <a href="http://jekyllrb.com" target="_blank">Jekyll</a>, refreshed.'
url:              https://fongandrew.github.io
...
{% endhighlight %}

Finally, the folders [_layouts](https://github.com/fongandrew/hydeout/tree/master/_layouts)
and [_includes](https://github.com/fongandrew/hydeout/tree/master/_includes)
contain boiler-plate HTML for building the website.
{% highlight yaml %}
$ ls -1 _layouts/
default.html
page.html
category.html
index.html
post.html
search.html
tags.html

$ ls -1 _includes/
category-links.html     custom-head.html        favicons.html           page-links.html         post-tags.html          sidebar-nav-links.html
comments.html           custom-icon-links.html  font-includes.html      pagination-newer.html   related_posts.html      sidebar.html
copyright.html          custom-nav-links.html   google-analytics.html   pagination-older.html   search-form.html        svg
custom-foot.html        disqus.html             head.html               post-meta.html          sidebar-icon-links.html tags-list.html
{% endhighlight %}


## Disqus Comments

Disqus integration is ready out of the box. Just add the following to
  your config file:

  ```yaml
  disqus:
    shortname: my-disqus-shortname
  ```

  If you don't want Disqus or want to use something else, override
  `comments.html`.

## Google Analytics

For Google Analytics support, define a `google_analytics` variable with your tracking ID in your config file.

## Getting a Custom URL

Once I got my blog up to speed on GitHub with the URL [azkh93.github.io](https://azkh93.github.io), it was easy to link my personal domain [azharkhan.in](https://azharkhan.in) to it. I use GoDaddy to host my domain, so I followed the instructions [here](https://hackernoon.com/how-to-set-up-godaddy-domain-with-github-pages-a9300366c7b).

I hope this blog post will help you get up to speed quickly with GitHub Pages, Jekyll, and Hydeout.
If you have any questions about my implementation, you can view my entire website on [GitHub](https://github.com/azkh93/azkh93.github.io) or leave a question below.

## More Links

Here are some links which helped me along the way:

* The official [Jekyll](http://jekyllrb.com) website, along with detailed [documentation](http://jekyllrb.com/docs/home).
* Documentation by GitHub about hosting static webpages with [GitHub pages](http://pages.github.com).
* The [poole](https://github.com/poole/poole) GitHub repository.
* The [Hydeout](https://github.com/fongandrew/hydeout) GitHub repository.
* The GitHub repository for my [personal website](https://github.com/azkh93/azkh93.github.io).
