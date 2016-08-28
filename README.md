# JEKYLL TEMPLATE

You wanna make a website with more than one page.

You're gonna use Jekyll, which is a website generator, and you're gonna host it on Github Pages, which is free, and which automatically runs Jekyll every time you make a change.

You might use prose.io as a sort of cms.

**This website is hosted (automatically) at https://amonks.github.io/jekyll-template **

## Contents

- [Getting Started](#getting-started)
- [Github Pages](#github-pages)
- [Jekyll](#jekyll)
    - [collections](#collections)
    - [frontmatter](#frontmatter)
    - [templates](#templates)
    - [markdown](#markdown)
    - [sass](#sass)
        - [tachyons](#tachyons)
            - [tricks](#tricks)
    - [data](#data)
    - [javascript](#javascript)
        - [turbolinks](#turbolinks)
- [prose.io](#proseio)
- [moving on from here](#moving-on-from-here)
    - [comments and dynamic content](#comments-and-dynamic-content)
    - [more media types](#more-media-types)

<!-- end toc 4 -->

## Getting Started

1. Sign up for github
2. fork this repository: your site will be online at http://[your-github-name].github.io/jekyll-template
3. rename it. You'll also have to change the `baseurl` in [`_config.yml`](https://github.com/amonks/jekyll-template/blob/gh-pages/_config.yml)
4. start editing

Now I'll start to explain how it works.

## Github Pages

[**github pages documentation**](https://pages.github.com/)

Github has a free product called GitHub Pages. Here's what it does:

If you have a github repository with a branch called 'gh-pages', it runs Jekyll on the contents of that branch, and then puts the output online at http://[your-github-name].github.io/[your-repo-name]. This one's at http://amonks.github.io/jekyll-template.

## Jekyll

[**jekyll documentation**](https://jekyllrb.com/docs/home/)

Jekyll looks at a folder like [this one](https://github.com/amonks/jekyll-template), and turns it into a website like [this one](http://amonks.github.io/jekyll-template).

You can use the `_config.yml` file to configure how it does that.

Any file that does not begin with an underscore or a dot is passed directly into the website, unless the first line of the file is three dashes: `---`. This file, `README.md` is a great example: http://amonks.github.io/jekyll-template.

Files that _do_ start with underscores are treated differently. [The `structure` page in Jekyll's docs](https://jekyllrb.com/docs/structure/) describes the basics of what each folder's for.

If a file _does_ start with three dashes, the frontmatter is removed, it's run through Markdown and Liquid, if it's a `.md` file, just Liquid if it's a `.html` file, or Sass if it's a `.scss` or `.sass` file, and the output becomes a page on your website.

I'll get into 'frontmatter', Markdown, Liquid, and Sass later.

### collections

[**jekyll collectionsdocumentation**](https://jekyllrb.com/docs/collections/)

Here, rather than using the default collection, `posts`, I've [defined three collections in `_config.yml`](https://github.com/amonks/jekyll-template/blob/gh-pages/_config.yml), `articles`, `pages`, and `media`.

If you put files in a collection, you can turn each file in that collection folder into a webpage, and make a list on your website somewhere with links to all the pages in the collection

### frontmatter

[**jekyll frontmatter documentation**](https://jekyllrb.com/docs/frontmatter/)

Look at the files in the `_articles` folder. Each one starts with a block of YAML or JSON between sets of three dashes, like this:

```
---
title: article title
---
```

that's called frontmatter. It's metadata about that page/file.

I've defined default frontmatter for each collection in [`_config.yml`](https://github.com/amonks/jekyll-template/blob/gh-pages/_config.yml).

- `title` is something I made up, that I use in the post template.
- `permalink` is built in. It determines the final url of the page.
- `layout` is built in. It determines which file in the `_layouts` folder is the template for that page.

There's more on permalinks on [the Permalinks page in the Jekyll docs](https://jekyllrb.com/docs/permalinks/)

### templates (liquid)

[**liquid documentation**](https://shopify.github.io/liquid/)

Jekyll uses a templating system called Liquid.

Here's the layout called `_layouts/article.html`:

```liquid
<!DOCTYPE html>
<html class="bg-yellow">
  {% include head.html %}
  <body>
    <div class="bg-purple yellow">
      {% include header.html %}
    </div>
    <div class="bg-white">
      <main>
        {% include article.html article=page %}
      </main>
    </div>
    {% include footer.html %}
    {% include scripts.html %}
  </body>
</html>
```

Let's look at 

```liquid
{% include head.html %}
```

That's a liquid tag called `include`. It means "put whatever's in [`_includes/head.html`](https://github.com/amonks/jekyll-template/blob/gh-pages/_includes/head.html) here".

```liquid
{% include article.html article=page %}
```

passes some data into that include. `page` is a special variable available in layouts that means "the page currently being layed out."

Here's the include at `_includes/article.html`

```liquid
<article>
  <h2>{{ include.article.title }}</h2>
  {% comment %}
    there's documentation about date formatting here:
    https://help.shopify.com/themes/liquid/filters/additional-filters#date
  {% endcomment %}
  <time datetime="{{ include.article.date | date: "%F" }}">{{ include.article.date | date: "%A, %B %-d, %Y"}}</time>
  {{ include.article.content }}
</article>
```

when we said `article=page`, it made the current page available to this include as `include.article`. That's where `include.article.title`, `include.article.date`, and `include.article.content` come from.

`content` is a thing pages have that means "the stuff after the frontmatter, run through markdown if it's a `.md` file."

`date: "$F"` is a "liquid filter".

### markdown

[**markdown documentation**](https://daringfireball.net/projects/markdown/syntax)

Markdown is a language for writing prose (articles or blog posts or stories or whatever, not like html structure so much) that turns into html. Here's an example:

```markdown
## An h2 header

Paragraphs are separated by a blank line. <em>You can use html in markdown, too.</em>

2nd paragraph. *Italic*, **bold**, and `monospace`. Itemized lists
look like:

* this one
* that one
* the other one
```

That turns into this html:

```html
<h2>An h2 header</h2>

<p>Paragraphs are separated by a blank line. <em>You can use html in markdown, too.</em></p>

<p>2nd paragraph. <em>Italic</em>, <strong>bold</strong>, and <code>monospace</code>. Itemized lists
look like:</p>

<ul>
<li>this one</li>
<li>that one</li>
<li>the other one</li>
</ul>
```

### css (sass)

[**the sass documentation**](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)

Markdown is a language that turns into html, sass is a language that turns into css.

Check out [`style.sass`](https://github.com/amonks/jekyll-template/blob/gh-pages/style.scss). It starts with three dashes, so jekyll processes it. Since it's a `.scss` file, that means it runs it through sass.

`.scss` is a version of sass that looks like css. `.sass` is a version of sass that doesn't. I like `.scss`.

The file uses two sass features, `@import`, and `@extend`.

`@import` is like liquid's `include`. It means "put all the sass from some file in `_sass` here".

`@extend` is kinda funky. It means "all the sass rules that these other things have, this thing should also have".

#### tachyons

[**tachyons documentation**](http://tachyons.io/docs/) 

tachyons is a big css file that includes classes for most of the possible css rules.

The tachyons approach is to design in your html, rather than your css.

css rules based on _what something is_ are bad. Here's an example:

```css
.contact-form .email-address {
  font-style: italic;
}
```

What if you want another email address to be italic? you have to write another css rule. You end up with specific css declarations for every individual thing on your website.

Instead, make css rules based on _what something looks like_.

```css
.fs-i {
  font-style: italic;
}
```

If you want all email addresses to be italic, you can make an include like this:

```html
<!-- _includes/email_address.html -->

<span class="fs-i">{{ include.address }}</span>
```

and then use it like this:

```liquid
{% include email_address.html address="a@monks.co" %}
```

An advantage is that you always know, when you're looking at your html, exactly what css is being applied. You don't have to remember "oh right I already defined an `.email` class with green text when I was working on the footer".

##### tricks

The easiest way to look up a tachyon class if you have the class name and you want to know what css it applies is to command-f in `_sass/_tachyons.scss`. 

The easiest way to look up a tachyon class if you know what css you want to apply but you don't know the class name is in [tachyons docs](http://tachyons.io/docs/)

The easiest way to find out what styles are being applied to something on your page and where they're coming from is to right click on it in your browser and select "inspect element" or "inspect".

### data

[**jekyll data documentation**](https://jekyllrb.com/docs/datafiles/)

`_data` is a special folder.

Check out [`_data/meta.yml`](https://github.com/amonks/jekyll-template/blob/gh-pages/_data/meta.yml). The values defined here are available in templates within `site.data.meta`. For example, in `_includes/header.html`, I use `{{ site.data.meta.title }}`. 

### javascript

You'll notice an include called [`_includes/scripts.html`](https://github.com/amonks/jekyll-template/blob/gh-pages/_includes/scripts.html) inserted before the end of the <body> tag in each layout. That's where you should put your javascript.

#### turbolinks

[**turbolinks documentation**](https://github.com/turbolinks/turbolinks)
turbolinks.js is the only script I've included in the template. It makes links within your website feel like they load faster by downloading the page and replacing only the parts that changed rather than doing a full page load.

## prose.io

[**prose.io documentation**](https://github.com/prose/prose/wiki/Getting-Started)

Prose.io is an editor for files in github.

You can set it up using the [`_prose.yml`](https://github.com/amonks/jekyll-template/blob/gh-pages/_prose.yml) file.

This template's prose.yml is set up with custom metadata fields for pages in the media collection. Fork this repository, edit a media page in prose.io, and click the Metadata icon on the right (it's supposed to look like a table, I think) to see what's up 

Here's a screenshot:

![prose.io metadata editor](http://f.monks.co/prose-io-yqI/prose-io.png)

I find it a lot easier to use than editing files in github, and it's extra helpful if you have multiple authors who don't want to have to know about code stuff.

## moving on from here

Your website probably isn't about `articles` and `media`. Or maybe it is. In any case, you'll want to customize it up, add your own jawns. What have you.

You'll probably at least want to switch up the categories and the site metadata.

Here are some things you might want to add:

### comments and dynamic content

often, sites keep track of comments in a database.

Jekyll generates "static sites", folders full of html files that are regenerated when you change the source files, _not_ every time someone visits the page.

You can still use javascript within those static html pages to do cool stuff. [disqus](https://disqus.com/) is a comments system that works on static sites: it includes javascript that downloads all the comments on the fly, rather than hardcoding them into your page.

You don't have to use disqus, but static sites that fetch dynamic content on the fly are a good approach. They're cheap to host, easy to cache, load quickly, and can use javascript and placeholders to gracefully handle errors downloading the dynamic content.

[firebase](https://firebase.google.com/pricing/) is database-and-api-as-a-service option from google with a generous free plan that can host any type of data you feel like, comments or otherwise. They provide a javascript library so you can easily reade and update your data from a website, but you'll have to deal with presenting that data on your page as html yourself.

### more media types

the approach I'm using for media is a good one. There's an include called `_includes/media.html` that delegates to other media includes based on a `type` passed in. Right now it supports youtube, vimeo, and images, but you could easily add three.js, processing, carousels with multiple media, or whatever.

On my own website, posts can specify an array of media in their frontmatter, the post layout calls the media include, and the media include can choose the appropriate include for the media type. It's easy to add to the system, and the layout and old posts don't need to change when I add new media types, or new features like an "aspect-ratio" argument on videos.

### custom domain

you can set a custom domain for a github-pages website. You'll have to change the baseurl in [`_config.yml`](https://github.com/amonks/jekyll-template/blob/gh-pages/_config.yml), you'll have to point your domain name to github's servers, and you'll have to add a CNAME file.

There's documentation about the process on [github's documentation website](https://help.github.com/articles/using-a-custom-domain-with-github-pages/).

