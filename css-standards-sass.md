# Transitioning to Sass for CSS

Sass sounds scary and difficult, but **it's really not,** and here's why: **you don't have to change anything** about the way you write CSS. If it's valid CSS, it's valid Sass -- you could literally use zero Sass syntax and be fine. But with Sass, once you get comfortable, you can also use extra stuff that makes it easier to write your CSS.

## Installing

First things first, Sass is a program, so let's install it. If you're on Windows, you need to install Ruby first. Fortunately, there's a [Ruby installer](http://rubyinstaller.org/) -- download and use it. When you do, tick the box to **add Ruby to your PATH environment variable.** If you're on a Mac, you already have Ruby, so you're fine.

You install Sass from the command-line, so open up your Windows `Command Prompt` or Mac `Terminal` program. Type:

    gem install sass

or

    sudo gem install sass

Once that finishes, **you're done.** Type `sass -v` if you want to make sure. If you get an error, type `ruby -v`, and then `gem -v`. If you get an error on either of those, then you don't have Ruby and/or RubyGems installed right, or their paths are not in your PATH variable, so fix that. Google if it's still not working.


## Getting Started

Here's the main difference up front: instead of creating and writing your code in a `*.css` file, you'll now create and write it in a `*.scss` file. Your same CSS, just a different file extension.

When you do that, Sass detects the CSS that you write into the `*.scss` file, processes it, and generates a standard `*.css` file from it. When you update your CSS in the `*.scss` file, Sass re-processes it and updates the `*.css` file with your changes. It's automatic.

Okay, so how does Sass do that?

When you begin your project, you'll need to set up Sass for it. Let's say your folder structure is:

    my-project
    |-- index.html
    |-- css
    |   +-- main.css
    +-- etc.

You'll need to do 3 quick things:

1. Change the file extension of `main.css` to `main.scss`. Worried that you've already written a bunch of styles into `main.css`? Don't be, doesn't matter.

2. Open your Windows `Command Prompt` or Mac `Terminal` program and navigate to your project's `/css` directory. Examples:

  `cd c:/wamp/www/my-project/css` (Windows/WAMP), **or**<br>
  `cd c:/xampp/htdocs/my-project/css` (Windows/XAMPP), **or**<br>
  `cd /Applications/MAMP/htdocs/my-project/css` (Mac/MAMP)
  
3. Still in your command prompt, type:

  `sass --watch main.scss:main.css --style expanded`

  (You can replace `main.scss` with whatever the actual name of your `scss` file is, and replace `main.css` with whatever name you want your resulting `css` file to be named.)

Once you run this command, you should see some extra stuff appear in your `/css` directory: a new `main.css` file, a `main.css.map` file, and a `/sass-cache` directory. This is good! You can now minimize -- **do not close** -- your command prompt. You are ready to use Sass.


### Heads up

It's important to remember that even though you now have a `*.css` file in your project, you should **only write code into the `*.scss` files.** The `*.css` files are automatically written **and rewritten** by Sass; if you add styles directly into them, they will inevitably get overwritten the next time Sass processes your code in the `*.scss` files. Write your CSS in the `*.scss` files only.


## Why Sass is worth it

Now that you're writing in Sass, you can use some of its very helpful features. Variables, nesting, parent selectors, extends, mixins, functions, arithmetic, aggregation, minification... there's a lot that Sass handles very simply. Let's start with variables.


### Variables

This is something that you can do with Sass:

    $dark-green: #285943;  // Declare a color value once, use it everywhere.
    $light-green: #77af9c; // No more having to remember hex and rgb values.

    h1 {
      color: $light-green;
      background: $dark-green;
    }
    .inner-div p a {
      color: $dark-green;
    }
    .inner-div p a:hover {
      color: $light-green;
    }

This is something else:

    $mobile: "max-width: 767px"; // Declare global breakpoints that everyone can reference.
    $tablet: "min-width: 768px";
    $desktop: "min-width: 1200px";

    div.sidebar {
      width: 100%;
      @media ($tablet) { // Make media queries easy to remember.
        width: 50%;
      }
      @media ($desktop) {
        width: 33.3333%;
      }
    }

What the what?! You should see 2 things there: super-easy media query use, AND the first example of nesting.

Sass will automatically process this and output:

    div.sidebar {
      width: 100%;
    }
    @media (min-width: 768px) {
      div.sidebar {
        width: 50%;
      }
    }
    @media (min-width: 1200px) {
      div.sidebar {
        width: 33.3333%;
      }
    }


### Nesting

Have some elements inside other elements on your page? Target them in Sass as easily as this:

    .nav {
        width: 100%;
        margin-bottom: 20px;

        ul {
            list-style: none;
            padding: 0;
            margin: 0;

            li {
                display: inline-block;
                margin: 0;
                padding: 0;
            }
        }
    }

HOLY SMOKES! Sass will automatically process this and output:

    .nav {
      width: 100%;
      margin-bottom: 20px;
    }
    .nav ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }
    .nav ul li {
      display: inline-block;
      margin: 0;
      padding: 0;
    }

Need to target an element's parent while you're nesting, or easily apply pseudo-stuff? Simple, use the `&` character:

    li {
        display: inline-block;

        &:nth-of-type(1) {      // this adds to the li
            margin-top: 20px;
            
            a {
                color: black;

                &:hover,        // these add to the a
                &:focus {
                    color: red;
                }
            }
        }
    }

Sass will output this as:

    li {
      display: inline-block;
    }
    li:nth-of-type(1) {
      margin-top: 20px;
    }
    li:nth-of-type(1) a {
      color: black;
    }
    li:nth-of-type(1) a:hover,
    li:nth-of-type(1) a:focus {
      color: red;
    }

That's pretty helpful, but maybe you're thinking, That didn't save me much typing. Okay, feast your eyeballs on extends and mixins.


### Extends

Ever have elements that are pretty much the same, but have one or two extra styles? Just use `@extend`. Example:

    .common {
      width: 33.3333%;
      padding: 20px 10px;
      font: 18px/1.2 'Open Sans', sans-serif;
    }
    .special {
      @extend .common;
      background: red;
    }

Now, `.special` will have all the styles that `.common` has, plus the red background you added to it.


### Mixins

Sass allows you to define a whole block of code and reference it by name. Sass calls this a "mixin." Here's a useful example:

    @mixin transition ($property, $duration: 0.3s, $easing: ease) {
      -webkit-transition: $property $duration $easing;
         -moz-transition: $property $duration $easing;
           -o-transition: $property $duration $easing;
              transition: $property $duration $easing;
    }
    
    div {
      .inner-div {
        opacity: 0;
      }
      &:hover .inner-div {
        opacity: 1;
        @include transition(opacity); // this single line executes the entire mixin above
      }
    }
    

This will output:

    div .inner-div {
      opacity: 0;
    }
    div:hover .inner-div {
      opacity: 1;
      -webkit-transition: opacity 0.3s ease;
         -moz-transition: opacity 0.3s ease;
           -o-transition: opacity 0.3s ease;
              transition: opacity 0.3s ease;
    }

You write the mixin once, you can use it everywhere. Now that clients want interactive animation out the wazzoo and the number of CSS3 transitions and transforms start multiplying, this becomes a huge time- and typing-saver, and prevents you from omitting a vendor prefix here and there.

Maybe now you're thinking, Okay that would be cool, but I can't plan out and write mixins for everything! Good news, dev: there's a large pre-built mixin library you can add into your project immediately! It's called Bourbon, and here's how to install it into your project:

1. Go back to your command prompt/terminal. Type:

  `gem install bourbon`
  
  (Hit `ctrl-c` first if you still have `sass --watch` running.)

2. Still in your command prompt, use `cd` to navigate back to your project's `/css` directory (or wherever the Sass for your project is running). Then type:

  `bourbon install`

3. You should now have a sub-directory named `/bourbon` added to your directory. Open up your `*.scss` file and type this on line 1:

  `@import 'bourbon/bourbon';`

Done! You now have a ton of mixins at your disposal, which you can use simply by calling `@include <the mixin name>` in your stylesheet. Boom.

Learn what mixins are available from Bourbon at [http://bourbon.io/docs/](http://bourbon.io/docs/)


### Color Functions

Ever wanted to make a color a little darker, but didn't know what hex value to use? Or soften a color that's just a little too sharp? With Sass, it's crazy-easy.

    div {
      color: #3cf;
    }
    .other-div {
      color: darken(#3cf, 10%);     // will be 10% darker than #3cf
    }
    .third-div {
      color: desaturate(#3cf, 40%); // will desaturate #3cf by 40%
    }

This will output:

    div {
      color: #3cf;
    }
    .other-div {
      color: deepskyblue;
    }
    .third-div {
      color: #5cb8d6;
    }

You can `adjust-hue()`, `lighten()`, `darken()`, `saturate()`, `desaturate()`, `grayscale()`, you can even `invert()` and find the `complement()` of a color. There's also `fade-in()` and `fade-out()` functions to make a color more or less opaque/transparent.


### Aggregation

The `@import` line for Bourbon above shows how easily Sass can aggregate multiple files into a single Sass file (granted, CSS can do this too). Lots of times it makes sense to separate out your CSS into different files -- so things are easier to find, grouped by similarity, etc. You do this in Sass with what they call "partials," which are just separate files that you name with an initial underscore.

Say you want to separate all your variable declarations into their own file (good idea!) so they're easy to find. You create a new file and name it `_variables.scss`. Notice the initial underscore.

You then include that file back into your `main.scss` file by adding

    @import 'variables'; // no underscore here

at or near the top of your `main.scss` file. Done!

A common strategy is to move all your styles out of the `main.scss` file, into as many Sass partials as makes sense, and then use a bunch of `@import`s in your `main.scss` to aggregate all the partials into a single file for Sass to process and convert into a single CSS file.


### Minification

You know it's best practice to minify your CSS for production, but what if you need to work on it later? Minifying and unminifying CSS for different environments is a nightmare. Sass solves this without even trying.

Go back to your command prompt/terminal. We're going to tweak our Sass "watch" command (if it's still running, hit `ctrl-c` first):

    sass --watch main.scss:main.css --style compressed

All we did was change the `--style` from `expanded` to `compressed`. But now, Sass will automatically minify all your styles in the `main.css` file so it's production-ready. Your `main.scss` file and partials, which are where you're doing all your development, stay un-minified so everything's easy to find and work on in the future. Best of both worlds.


### So what goes up to production?

Just the `*.css` file(s) that Sass creates. All of your Sass files (and sub-directories and partials and mixins) stay in your development environment. Sass doesn't need to be part of your actual website/production server at all. It is strictly a development tool. Awesome!

As far as the HTML goes, you link to the `*.css` file(s) just like you always have:

    <link rel="stylesheet" href="main.css">

One caution: Some browsers may return an error that it can't find a `*.map.css` file. This is because Sass adds a reference to it at the bottom of your `*.css` file, something like:

    /*# sourceMappingURL=main.css.map */

To remove the error, just **delete** this comment line from your production CSS file(s). It's for [mapping](https://developer.chrome.com/devtools/docs/css-preprocessors#toc-how-css-source-maps-work), which you don't need in production.

That's it, you're ready to start using Sass in your next project. If other devs will be working on your site too, make sure they know what to do (and not do -- no writing into the `*.css` files!). Good luck!
