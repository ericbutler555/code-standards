# Using SASS for CSS

This document already assumes you've [installed Sass](http://sass-lang.com/install) (and [installed Ruby](http://rubyinstaller.org/) first if you're on Windows) on your computer.

Sass sounds scary and difficult, but **it's really not,** and here's why: you don't have to change anything about the way you write CSS. Valid CSS is valid SASS. But with Sass, once you get comfortable, you can also use extra stuff that makes it easier to write your CSS.

Here's the main physical difference: instead of creating and writing your code in a `*.css` file, you'll now create and write it in a `*.scss` file. Your same CSS, just a different file extension.

When you do that, Sass detects the CSS that you write into the `*.scss` file, processes it, and generates a standard `*.css` file from it. When you update your CSS in the `*.scss` file, Sass re-processes it and updates the `*.css` file with your changes. It's automatic.

Okay, so how does Sass do that?

When you begin your project, you'll need to set up Sass for it. Let's say your folder structure is:

    my-project
    |-- index.html
    |-- css
    |   +-- main.css
    |-- img
    +-- js

You'll have to do 3 quick things:

1. Change the file extension of `main.css` to `main.scss` (or start with a `main.scss` file to begin with).

2. Open your Windows `Command Prompt` (or Mac `Terminal`) program and navigate to your project's `css` directory. Examples:

  `cd c:/wamp/www/my-project/css` (Windows/WAMP) or<br>
  `cd c:/xampp/htdocs/my-project/css` (Windows/XAMPP) or<br>
  `cd /Applications/MAMP/htdocs/my-project/css` (Mac/MAMP)
  
3. Still in your command prompt, type:

  `sass --watch main.scss:main.css --style expanded`

  (Replace `main.scss` with whatever the actual name of your `scss` file is, and replace `main.css` with whatever name you want your resulting `css` file to be named.)

Once you run this command, you should see some extra stuff appear in your `/css` directory: a new `main.css` file, a `main.css.map` file, and a `/sass-cache` directory. This is good! You can now minimize -- **do not close** -- your command prompt. You are ready to use Sass.


## Why Sass is worth it

Now that you're writing Sass, you can use some of its very helpful features. Variables, nesting, parent selectors, extends, mixins, functions, arithmetic, aggregation, instant minification... there's a lot that Sass handles very simply. Let's start with variables.


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

    $phone: "max-width: 553px";
    $phablet: "min-width: 554px";
    $tablet: "min-width: 768px";
    $tablet-wide: "min-width: 992px";
    $laptop: "min-width: 1200px";
    $desktop: "min-width: 1600px";

    div.sidebar {
      width: 100%;
      @media ($tablet) {
        width: 50%;
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

HOLY SMOKES! Sass would automatically process this and output:

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
      
      &:before {
        content: "whatever";
      }
      
      a {
        color: black;
        
        &:hover,
        &:focus {
          color: red;
        }
      }
    }

Sass will output this as:

    li {
      display: inline-block;
    }
    li:before {
      content: "whatever";
    }
    li a {
      color: black;
    }
    li a:hover,
    li a:focus {
      color: red;
    }

Maybe you're thinking, Okay, that's kinda cool, but that didn't save me much coding. Okay, feast your eyeballs on extends and mixins.


### Extending

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

Now, `.special` will have all the styles that `.common` has, plus the background style you added to it.


### Mixins

Sass allows you to define a whole block of code and reference it by name. Sass calls this a "mixin." Here's a very useful example:

    @mixin transform ($values) {
      -webkit-transform: $values;
         -moz-transform: $values;
          -ms-transform: $values;
           -o-transform: $values;
              transform: $values;
    }
    
    div {
      .inner-div {
        opacity: 0;
      }
      &:hover .inner-div {
        opacity: 1;
        @include transform(translateX(-10px) scale(1.05)); // this single line executes the entire mixin above
      }
    }
    

This will output:

    div .inner-div {
      opacity: 0;
    }
    div:hover .inner-div {
      opacity: 1;
      -webkit-transform: translateX(-10px) scale(1.05);
         -moz-transform: translateX(-10px) scale(1.05);
          -ms-transform: translateX(-10px) scale(1.05);
           -o-transform: translateX(-10px) scale(1.05);
              transform: translateX(-10px) scale(1.05);
    }

You write the mixin once, you can use it everywhere. Now that clients want interactive animation out the wazzoo and the number of CSS3 transitions and transforms start multiplying, this becomes a huge time- and typing-saver, and prevents you from omitting a vendor prefix here and there.

Maybe now you're thinking Okay, that would be cool, but I don't want to have to plan out and write mixins for everything! Good news, dev: there's a complete mixin library that you can add into your project immediately! It's called Bourbon, and here's how to install it into your project:

1. Go back to your command prompt/terminal. Type:

  `gem install bourbon`
  
  (Hit `ctrl-c` first if you still have `sass --watch` running.)

2. Still in your command prompt, use `cd` to navigate back to your project's `/css` directory (or wherever the Sass for your project is running). Then type:

  `bourbon install`

3. You should now have a sub-directory named `/bourbon` added to your directory. Open up your `*.scss` file and type this on line 1:

  `@import 'bourbon/bourbon';`

Done! You now have a large mixin library at your disposal, which you can use simply by calling `@include <the mixin name>` in your stylesheet. Boom.

Learn what mixins are available from the Bourbon documentation: [http://bourbon.io/docs/](http://bourbon.io/docs/)


### Aggregation

The `@import` line for Bourbon shows how easily Sass can aggregate multiple files into a single Sass file (okay, CSS can do this too). Lots of times it makes sense to separate out your CSS into different files, so things are easier to find, grouped by similarity, etc. You do this in Sass with what they call "partials," which is really just separate files that you name with an initial underscore. Example:

Say you want to separate all your variable declarations into their own file (good idea!) so they're easy to find. You create a new file and name it `_variables.scss`. Notice the initial underscore.

You then include that file back into your `main.scss` file by adding

`@import 'variables';`

at or near the top of your `main.scss` file. Done!

You can (and depending on the project, maybe *should*) separate your CSS into as many Sass partials as makes sense, and then only use `main.scss` as a way to aggregate all the partials into a single file for Sass to process and convert into a single CSS file.


### Minification

You know it's best practice to minify your CSS for production, but what if you have to do further development? Minifying and unminifying CSS for different environments is a nightmare. Sass solves this without even trying.

Go back to your command prompt/terminal. We're going to tweak our Sass "watch" command (if it's still running, hit `ctrl-c` first):

    sass --watch main.scss:main.css --style compressed

All we did was change the `--style` from `expanded` to `compressed`. But now, Sass will automatically minify all your styles in the `main.css` file so it's production-ready. Your `main.scss` file, which is where you're writing all your styles, stays un-minified. Best of both worlds.


### So what goes up to production?

Just the `*.css` file(s) that Sass creates. All of your Sass files (and sub-directories, and Bourbon) stay in your development environment. Sass doesn't need to be part of your actual website/production server at all. It is strictly a development tool. Awesome!

One caution: Some browsers may return an error that it can't find a `*.map.css` file. This is because Sass adds a reference to it at the bottom of your `*.css` file in something like:

    /*# sourceMappingURL=main.css.map */

To remove the error, either delete this comment line from your CSS file, or push up your `*.css.map` file to production too. Your choice on which is better.
