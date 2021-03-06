# CSS Standards

First, why have standards?

1. CSS tends to get really long and messy for an entire website.

2. Developers need to be able to understand and work comfortably on each other's CSS; it keeps you from being the lone dev working 14-hour shifts on an emergency turnaround project. It also allows you to revisit a site 9 months later and quickly figure out what you, or someone else, was trying to do.

3. You are not the only person who'll ever see and work on your code. Standards make the next person's job a little less hard.

With this in mind, when writing your own CSS, please follow these guidelines:



## General Guidelines


### Mobile First: Media Queries, Breakpoints

Write media queries from a **<a href="http://ux.stackexchange.com/a/35317" target="_blank">mobile-first</a>** perspective, which means using `min-width` for media queries, **not** `max-width`. This means that any CSS rules that are **not** wrapped in a media query should be targeting mobile devices, and anything targeting a wider/desktop layout should be wrapped in a media query for the specific breakpoint that makes sense.

This also means that in terms of strategy, you should **start by styling/laying-out your webpages for the mobile layout.** Starting with the mobile layout and expanding it for desktop is almost always faster and more successful than starting with the desktop layout and trying to reduce it down to fit the mobile styles.

**Do not write one-off media queries throughout a CSS file.** Each breakpoint/media query should be written as a single wrapper, with all styles for that breakpoint included inside it. This makes it very easy to see what styles are being applied (or modified) at a given breakpoint.

Each media-query block should be put in order from smallest viewport to largest. In other words, all style rules that are **not** wrapped in a media query should come **first,** then the first media-query block should be the one that targets the least-wide viewport/breakpoint, then the next-wide, and so on.

This ordering is important for 2 reasons: 1. So that no style accidentally overrides another style, and 2. So that it is easy to find and identify each media-query block.

<a href="http://v4-alpha.getbootstrap.com/layout/grid/#grid-options" target="_blank">Bootstrap 4</a> uses the following media queries, **which you should mirror to ensure consistency** if you (or anyone in the future) use Bootstrap on the project:

Naked rules (not wrapped in any media query, for phone viewports):

    0px - 575px

"Small" (phablet landscape) viewports and up:

    @media (min-width: 576px) { ... }

"Medium" (tablet portrait) viewports and up:

    @media (min-width: 768px) { ... }

"Large" (tablet landscape) viewports and up:

    @media (min-width: 992px) { ... }

"Extra large" (laptop) viewports and up:

    @media (min-width: 1200px) { ... }

You may also want an extra query above that, since the desktop computers that many people use are nearly 2000 px wide:

    @media (min-width: 1600px) { ... }

You probably won't need all of these. Only use the specific media queries you need to get the right layouts for the project. But these should be your expected breakpoints.


### Custom Media Queries

Sometimes you have to fix a certain element's styles at a certain range of viewport widths. These custom media queries should always be included **after every other media query** so that they do not get overruled. They should also specify both a `min-width` **and** a `max-width` in the media query, so that the fix doesn't accidentally bleed into any other viewport width ranges.

Example:

    @media (min-width: 768px) and (max-width: 840px) {

      .my-element {
        padding-left: 0;
      }

    } /* end media query 768-840px */


### Separate Files

Unless you're working on a small site, separate your CSS into different files: one for global styles, and then one for each webpage. If the styling is complex/lengthy, you may even create 2 files per page: one for the default (mobile-first) rules, and one for the rules wrapped in media queries (see the "Media Queries" section for recommended breakpoints). This way, you and other devs won't have to hunt for styles across a single, 5000-line stylesheet, **and** different devs can more easily work on layouts simultaneously. This also allows you to load individual stylesheets conditionally on only the pages they relate to. (You can always consolidate these files into a single stylesheet for the live site.)


### Third-Party CSS

When using 3rd-party CSS like <a href="https://necolas.github.io/normalize.css/" target="_blank">Normalize</a>, <a href="http://getbootstrap.com" target="_blank">Bootstrap</a>, an <a href="https://fortawesome.github.io/Font-Awesome/" target="_blank">icon/font stylesheet</a>, or a JS library's associated styles, use (or [create](#minifying)) the **minified** version of their stylesheet(s).

**Do not** write style changes into the 3rd-party files. Instead, simply override them in your own custom CSS file(s). Add a block comment above it if that's helpful. This way, someone else who starts working on your files will see that you've altered the core styling, and if they ever swap-in a newer version of the third-party files, they don't lose your custom changes.


### Minifying

For production, create a minified (and maybe aggregated) copy of your CSS file(s). Use <a href="http://cssminifier.com" target="_blank">CSSminifier.com</a> or similar to do so. Name it `[thisFileName].min.css`, and point your pages to that new minified file. KEEP the un-minified version(s) for ongoing development. When changes are made to the development file(s), re-minify, test, and replace the minified version(s), and push them up to production.



## Rule Formatting Guidelines


### Specificity

Specificity is first because it's really important: Do not set rules for selectors that are broad enough that they could spill out to any other use of the same selector. Be especially careful setting styles for naked elements (`body`, `p`, `a`, `h3`, `ul`, `div`, `nav`, etc.) since these styles will very likely need to be manually re-set when a future site addition requires using those elements in a different way. A good rule of thumb is that broad rules should **only** be set in order to "normalize" or "reset" browser default styles.

Examples:

    /* BAD: */
    body {
      background-color: beige; /* any page that doesn't have this bg will have to explicitly override this style */
    }
    a {
      color: #000; /* this will color normal text links, but also links in your nav menu, footer, etc. */
    }
    .content p {
      padding: 0 20px; /* any paragraph text anywhere in the content div will have side padding applied to it */
    }

    /* BETTER: */
    body.home-page {
      background-color: beige; /* limits the rule to a page-specific class instance */
    }
    a {
      color: inherit; /* overrides the browser's (and Bootstrap's) default blue, inherits its parent's color */
    }
    .content > p {
      padding: 0 20px; /* only paragraph text that is a direct child of the content div will be given padding */
    }


### Readability

When naming classes, IDs, etc., use names that obviously relate to the content of the element (like `.intro-text`) or a widely-applicable style setting (like `.clearfix`), and are **easy for humans to read.** Don't create arbitrary, non-obvious shortenings or acronyms.

Not as important, but nice for consistency: **Use lowercase and hyphens for class names, and camelCase for ID names.** Avoid underscores on both.


### Selectors

(e.g. `h1`, `#topNav`, etc.): Put each on its own line, then a space, then the opening curly bracket. For multiple selectors, add a comma after each element, then a line break.

Example:

    button.main-button,
    button.secondary-button {
      /* declarations here */
    }


### Declarations

(e.g. `color: #000;`, etc.): Always on their own line. Preceded by 2 spaces. 1 space after the colon. Always end with a semicolon.

Example:

    .intro-section {
      width: 100%;
      padding: 20px 0 40px;
      background-color: #ddd;
    }

**One Exception:**

If the entire rule is just for a single selector and declaration, especially when setting a bunch of very similar rules together, write it as a single line:

Example:

    .item#item1 { background-color: red; }
    .item#item2 { background-color: yellow; }
    .item#item3 { background-color: green; }
    .item#item4 { background-color: blue; }


### Closing Curly Braces

Always put closing curly braces on their own line, aligned vertically below the selectors.

Examples can be seen in the Selectors and Declarations guidelines above.


### Grouping

Group multiple rules that style a similar element or section together, and add a heading or sub-heading indicating what the common element or section is (see Block Comments, below).


### Block Comments

Add block comments to identify what a group of selectors is for: what pages, what sections, what elements.

Example:

    /**********
     *
     * GLOBAL NAVIGATION MENU
     *
     **/

    #topNav {
      /* whatever */
    }
    .nav-icon {
      /* whatever */
    }

    /**********
     *
     * PAGE: HOMEPAGE
     *
     **/

     .hero-banner {
       /* whatever */
     }


### Inline Comments

This one really needs to be done more: Add inline comments to indicate what certain declarations are trying to accomplish if it's not obvious. Especially for properties like `position`, `display`, `z-index`, negative margin, etc.

Example:

    .my-element {
      display: none; /* hide by default, show w/ jquery */
      position: relative; /* to contain child elements using position:absolute */
      z-index: 9; /* to keep above normal elements, but below sticky footer */
      margin: -15px -15px 0 0; /* to offset padding on parent's top-right corner */
    }


### Line Spaces

No need for full line spaces between rules; the closing curly quote already kinda provides one. But can be helpful to separate a group of related rules from other rules. **Do** add line spaces before block-comments.


### Indents

Do not indent rules, aside from the 2 spaces before the declarations inside a block. Indenting the blocks themselves will most likely just be confusing to others. Logical naming conventions and block-comment grouping should provide enough context.

Example:

    .container {
      padding: 20px;
    }

        /* DON'T INDENT LIKE THIS: */
        .container-inner {
          text-align: center;
        }


### Browser Support, Vendor Prefixes

Use vendor prefixes to make sure your CSS is supported as far back as reasonably possible. In particular, `transition`, `transform`, and `gradient` declarations **need vendor prefixes,** as do (lesser-used) `calc`, `flex`, `appearance`, `placeholder`, `columns`, `animation`, and `keyframes`. Prefixes for `border-radius` and `box-shadow` are optional.

To check browser support and see what vendor prefixes are needed for a property, **make a habit** of checking <a href="http://caniuse.com/" target="_blank">caniuse.com</a>.

Examples of vendor prefixing:

    .my-element {
      -webkit-transition: -webkit-transform 0.4s;
         -moz-transition:    -moz-transform 0.4s;
           -o-transition:      -o-transform 0.4s, transform 0.4s;
              transition: -webkit-transform 0.4s, transform 0.4s;
    }
    .my-element.enlarged {
      -webkit-transform: scale(1.05);
         -moz-transform: scale(1.05);
          -ms-transform: scale(1.05);
           -o-transform: scale(1.05);
              transform: scale(1.05);
    }
    .my-element2 {
      background-image: -webkit-linear-gradient(black, white);
      background-image:    -moz-linear-gradient(black, white);
      background-image:      -o-linear-gradient(black, white);
      background-image:         linear-gradient(black, white);
    }


### Print Styles

If there is enough time, or the client requires it, don't forget to check if your webpages look okay printed out. Hit Cmd/Ctrl+P (or File>Print) on each page of your site and see if any elements need to be hidden, or made smaller, or darker, or unimportant images, banners, or navigation menus removed, etc.

If so, create a new media query and include it **at the very bottom** of your stylesheet:

    @media print {

      * {
        color: #000; /* all text should be black for easiest printing, reading */
      }
      .no-print {
        display: none !important; /* consider a helper class to add to elements you don't want to print */
      }

    } /* end print media query */

People **do** still print out things they find on the internet, so try to make sure it isn't a mess when they do.


### Base CSS File

In this repo there is a `base.min.css` file that combines and minifies the latest versions of <a href="https://necolas.github.io/normalize.css/" target="_blank">Normalize</a> and <a href="https://github.com/h5bp/html5-boilerplate/blob/master/dist/css/main.css" target="_blank">HTML5Boilerplate</a>'s styles. This is a good little stylesheet to include in your webpages before writing any of your own CSS, as it creates some base-level consistency across browsers and offers a few helper classes (like `.clearfix`) and some basic `@print` styles.

Include this file in your project or, even better, copy and paste these styles into the top of your global (or aggregated) stylesheet to save the browser an extra HTTP request. Just make sure they come first so they don't override any of your own CSS.
