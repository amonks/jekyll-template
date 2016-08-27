# JEKYLL TEMPLATE

You wanna make a website with more than one page.

You're gonna use Jekyll, which is a website generator, and you're gonna host it on Github Pages, which is free.

## Contents

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

<!-- end toc 4 -->

## Github Pages

Github has a free product called GitHub Pages. Here's what it does:

If you have a github repository with a branch called 'gh-pages', it runs Jekyll on the contents of that branch, and then puts the output online at http://[your-github-name].github.io/[your-repo-name]. This one's at http://amonks.github.io/jekyll-template.

## Jekyll

Jekyll looks at a folder like [this one](https://github.com/amonks/jekyll-template), and turns it into a website like [this one](http://amonks.github.io/jekyll-template).

You can use the `_config.yml` file to configure how it does that.

Any file that does not begin with an underscore or a dot is passed directly into the website, unless the first line of the file is three dashes: `---`. This file, `README.md` is a great example: http://amonks.github.io/jekyll-template.

Files that _do_ start with underscores are treated differently. [The `structure` page in Jekyll's docs](https://jekyllrb.com/docs/structure/) describes the basics of what each folder's for.

If a file _does_ start with three dashes, the frontmatter is removed, it's run through Markdown and Liquid, if it's a `.md` file, just Liquid if it's a `.html` file, or Sass if it's a `.scss` or `.sass` file, and the output becomes a page on your website.

I'll get into 'frontmatter', Markdown, Liquid, and Sass later.

### collections

Here, rather than using the default collection, `posts`, I've [defined three collections in `_config.yml`](#), `articles`, `pages`, and `media`.

If you put files in a collection, you can turn each file in that collection folder into a webpage, and make a list on your website somewhere with links to all the pages in the collection

### frontmatter

Look at the files in the `_articles` folder. Each one starts with a block of YAML or JSON between sets of three dashes, like this:

```
---
title: article title
---
```

that's called frontmatter. It's metadata about that page/file.

I've defined default frontmatter for each collection in [`_config.yml`](#).

- `title` is something I made up, that I use in the post template.
- `permalink` is built in. It determines the final url of the page.
- `layout` is built in. It determines which file in the `_layouts` folder is the template for that page.

There's more on permalinks on [the Permalinks page in the Jekyll docs](https://jekyllrb.com/docs/permalinks/)

### templates

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

That's a liquid tag called `include`. It means "put whatever's in [`_includes/head.html`](#) here".

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

### sass

Markdown is a language that turns into html, sass is a language that turns into css.

Check out [`style.sass`](#). It starts with three dashes, so jekyll processes it. Since it's a `.scss` file, that means it runs it through sass.

`.scss` is a version of sass that looks like css. `.sass` is a version of sass that doesn't. I like `.scss`.

The file uses two sass features, `@import`, and `@extend`.

`@import` is like liquid's `include`. It means "put all the sass from some file in `_sass` here".

`@extend` is kinda funky. It means "all the sass rules that these other things have, this thing should also have".

#### tachyons

[`tachyons`](http://tachyons.io/docs/) is a big css file that includes classes for most of the possible css rules.

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

`_data` is a special folder.

Check out [`_data/meta.yml`](#). The values defined here are available in templates within `site.data.meta`. For example, in `_includes/header.html`, I use `{{ site.data.meta.title }}`. 

