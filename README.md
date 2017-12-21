## &lt;uitk-loader&gt; and Custom HTML Tags

# Intro
Product Design recently asked for a new version of the UITK Loader and since we had just finished The View Showdown (more on that in a future post), we were curious to see how it could be built with one of the newer view libraries like React.

We already rebuilt UITK Alert with these libraries as a POC, so we knew what it took for a medium-sized component, but we hadn’t tried to create a small component like Loader. It became apparent that the solution in every case would unfortunately be over-engineered.

## Over-engineering On the Rise
This is what the JavaScript world looks like to me:

![Image](src)

Hyper. Chaotic. Like an insatiable mutt running back and forth revisiting places like its never been there before.

The trouble comes when you’re using one of these new libraries and you have something that isn’t a component, something like UITK’s Loader. It’s definitely a UI “thing”, it has a unique identity and purpose, but it doesn’t really do anything, not anything substantial. Loader is like a micro component.

In the React world you “component all the things!” so a Loader would be done in React (numerous examples online, BEX as well). No one in the React community frowns upon 10, 20, or 60 lines (seriously.) of JavaScript just to produce a single <span>, which is what Loader is. There’s even a special React API for these “pure” components. This is not good.

_“Just say NO to drugs over-engineering!”_

-McGruff, the Engineering Dog

## Scaling Down
React and some of the other libraries make it hard or impossible to scale down, so what do we do then for Loader?

While rebuilding Alert with Polymer, Slim, and Riot I noticed that, despite me failing to write working JavaScript, the components still rendered. Broken, but the initial state rendered. I was intrigued and it got me thinking about how WebComponents (Polymer, Slim, and Riot are close to WebComponents) can server-render without Node since they’re real HTML…

![Image](src)

And if they render without working JavaScript, then they’d certainly render with no JavaScript at all, which means we’d be back to basic HTML and CSS, but with the look of a true component. Except it wouldn’t be a React component or even a WebComponent – it’d simply be a custom HTML tag.

# Custom HTML Tags
Now that we’re IE11+ it makes a lot of sense to consider custom HTML tags.

You’ll soon see an example of our first custom tag on the Doc Site:
```
<uitk-loader></uitk-loader>
```
This replaces the older Loader implementation, which looked like:
```
<span class="loader"></span>
```
This new tag approach is just HTML. Really, plain old HTML. There’s no 3rd-party library or polyfill to download, no UITK magic or Handlebars abstraction, no hacks, nothing. It’s a custom or “unknown” HTML tag and browsers will download, parse, and style them like normal tags and we think they’re a great approach to these micro components.

Here’s what a few others would look like:
```
<uitk-box>...</uitk-box>
<uitk-row>...</uitk-row>
<uitk-icon></uitk-icon>
```
No doubt you’ve noticed two differences here compared to current UITK: prefixed tag names and no classes.

Prefixed tags are required for WebComponents, but not custom tags. We’re going to follow this rule anyway because it’s a good practice and will make it possible for a custom tag to grow up and become a real WebComponent while still remaining backwards-compatible, and the prefix stands out nicely when looking at code, “That’s clearly a UITK component right there!”

Classes are good, but custom tags are just…better. Some differences between custom tags and their benefits compared to classes include:

**Easily identifiable tag** It can be hard to distinguish your app’s code from UITK’s (even with a uitk prefix):



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
