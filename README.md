## &lt;uitk-loader&gt; and Custom HTML Tags

# Intro
Product Design recently asked for a new version of the UITK Loader and since we had just finished The View Showdown (more on that in a future post), we were curious to see how it could be built with one of the newer view libraries.

We already rebuilt UITK Alert with these libraries as a POC, so we knew what it took for a medium-sized component, but we hadn’t tried to create a small component like Loader. It became apparent that the solution in every case would unfortunately be over-engineered.

## Over-engineering On the Rise
This is what the JavaScript world looks like to me:

![Image](src)

Hyper. Chaotic. Like an insatiable mutt running back and forth revisiting places like its never been there before.

The trouble comes when you’re using one of these new libraries and you have something that isn’t a component, something like UITK’s Loader. It’s definitely a UI “thing”, it has a unique identity and purpose, but it doesn’t really do anything, not anything substantial. Loader is like a micro component.

In the React world you “component all the things!” so a Loader would be done in React (numerous examples online, BEX as well). No one in the React community frowns upon 10, 20, or 60 lines (seriously.) of JavaScript just to produce a single <span>, which is what Loader is. There’s even a special React API for these “pure” components. This is not good.

_“Just say NO to ~~drugs~~ over-engineering!”_

-McGruff, the Engineering Dog

## Scaling Down
React and some of the other libraries make it hard or impossible to scale down, so what do we do then for Loader?

While rebuilding Alert with Polymer, Slim, and Riot I noticed that, despite me failing to write working JavaScript, the components still rendered. Broken, but the initial state rendered. I was intrigued and it got me thinking about how WebComponents (Polymer, Slim, and Riot are close to WebComponents) can server-render without Node since they’re real HTML…

![Image](src)

And if they render without working JavaScript, then they’d certainly render with no JavaScript at all, which means we’d be back to basic HTML and CSS, but with the look of a true component. Except it wouldn’t be a React component or even a WebComponent – it’d simply be a custom HTML tag.

# Custom HTML Tags
Now that we’re IE11+ it makes a lot of sense to consider custom HTML tags.

You’ll soon see an example of our first custom tag on the Doc Site: `<uitk-loader></uitk-loader>`

This replaces the older Loader implementation, which looked like: `<span class="loader"></span>`

This new tag approach is just HTML. There’s no 3rd-party library or polyfill to download, no UITK magic or Handlebars abstraction, no hacks, nothing. And when it comes to code, _nothing_ is better than something.

It works because browsers can download, parse, and style custom or “unknown” HTML tags like normal tags and we think they’re a great approach to building micro components.

Here’s what a few others would look like:
```
<uitk-box>...</uitk-box>
<uitk-row>...</uitk-row>
<uitk-icon></uitk-icon>
```
No doubt you’ve noticed two differences here compared to current UITK: 
prefixed tag names and no classes

Prefixed tags are required for WebComponents, but not custom tags. We’re going to follow this rule anyway because it’s a good practice and will make it possible for a custom tag to grow up and become a real WebComponent while still remaining backwards-compatible, and the prefix stands out nicely when looking at code, “That’s clearly a UITK component right there!”

Classes are good, but custom tags are just…better. Some differences between custom tags and their benefits compared to classes include:

**Easily identifiable tag** It can be hard to distinguish your app’s code from UITK’s (even with a uitk prefix):
```
<div id="foo" class="bar box baz">...</div>
<div id="foo" class="bar uitk-box baz">...</div>

<!-- vs. -->

<uitk-box id="foo" class="bar baz">...</uitk-box>
```
**Improved semantics** Custom tags really communicate a strong meaning and make for easier testing:
```
<!-- Bad --> 
<div class="content-container-class">...</div> 

<!-- Good --> 
<div class="box">...</div> 

<!-- Even better --> 
<uitk-box>...</uitk-box>
```
**Cleaner code and a clear API:**
```
<i class="icon icon-traveler"></i> 

<!-- vs. --> 

<uitk-icon name="traveler"></uitk-icon>
```
**Custom attributes** really open up a lot of possibilities:
```
<uitk-badge count="1"></uitk-badge>
```
🏆 _Bonus points if you leave a comment explaining how Badge’s count displays without JavaScript_

That same example above could easily evolve into a WebComponent without requiring any refactoring. Check it out:

v1, custom tag:
```
<uitk-badge count="1"></uitk-badge>
```
v2, uitk-badge becomes a WebComponent and exposes more functionality:
```
<!-- Valid WebComponent, still works, no impact after v2 is released -->
<uitk-badge count="1"></uitk-badge>

// Begin using new API if desired
badge.updateCount(2);
badge.clear();
```
Are you starting to see the value here?

One other nice benefit is all your existing tooling still works: syntax highlighting, Emmet (hope you’re using it!), other IDE features, testing tools, and all browsers’ developer tools (no Chrome-only plugins required like some libraries).

In summary, we can create high-level component APIs using the most basic web technology – HTML – instead of over-engineering with the flavor-of-the-day.

# So then, what about all those view libraries?





```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/jfbrennan/blog/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
