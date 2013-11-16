# Sassaparilla

A quick start template for (hopefully) better websites.

## Overview

Sassaparilla is a set of default rules and style that starts everything we do at [fffunction](http://fffunction.co) in a consistent manner. 

It's not a boilerplate or a theme.

Sarsaparilla on the other hand, is a nice refreshing root beer. Perhaps crack one open while you're reading this.

---

## Getting Sass(y) with Compass

Sassaparilla uses the power of [Sass](http://Sass-lang.com) via SCSS and [Compass](http://compass-style.org) to create flexible stylesheets that we can re-use and add to over time.

Compass is a library of common elements, equations and helper styles that is kept up-to-date by a community of developers and is useful for rapid development and leaner code.

Both Sass and Compass are Ruby Gems and will need to be installed via command line or terminal (if you're on a mac you're already rocking Ruby baby).

You can compile using a GUI like Codekit or go hardcore and just use command line. Either way it's pretty easy to get up and running.

You'll find documentation on both the Sass and Compass websites on how to install and use these tools. If you plan on installing Compass (and you'll need to), then you shouldn't need to install Sass separately. 

To install compass on a Mac open Terminal and type:

``` sh
$ gem update --system
```

This will update the system. Then

``` sh
$ gem install compass
```

To install compass

If you have trouble, try using the 'sudo commands' (with care) to access the correct level of permissions. E.g

``` sh
$ sudo gem update --system 
$ sudo gem install compass
```

--- 

## CSS set-up

Sassaparilla follows a specific cascade, and makes use of several SCSS files, each of which serves a different purpose.

Note: If a file begins with an underscore, that file will not compile to its own css file. Files with underscores are considered includes.

### Screen.scss

This contains the bulk of your styles, as well as pulling together the other partial stylesheets into one css file. By and large you will be doing most of your work within this stylesheet.

### Reset.scss

This contains a simple reset for html elements. It assumes your webpage will have forms, so resets these too. If your site doesn't use forms, you can remove these resets.

### Settings.scss

This contains all the variables for the site. Things such as colours and font sizing live here. This file is for defining your core set-up and makes use of compass' baseline and font-size measures. More on that later.

### Mixins.scss

Mixins contains any user made mixins for the project. It includes some simple ones to get you started. Add more as needed.

### Typography.scss

This file contains the core typesetting for the site. It relies on variables set-up in the settings.scss file. If you don't wish to use compass's rhythm method, you can leave this out.

### Forms.scss

Contains default form elements and standard styling. If you're not using forms you can safely leave this out.

### Icons.scss

Easy spriting, with retina support built in. Simply include _icons.scss, and then put your normal and double size pngs into `/assets/img/ico` and `/assets/img/ico2x` folders. Compass will create image sprites for you, then add any element with `ico` and `ico-[image-name]` classes for spriting loveliness.

--- 

## Compass rhythm and leading

Sassaparilla tries to make leading and spacing as easy as possible, whilst writing accessible code. We're not mad keen on tricky maths, but we like to write in ems. Luckily, compass can handle this for us with a few smart defaults, tricks and a few commands. Here's how:

#### In settings.scss

##### Setting up your base-font-size. 
This defaults to 16px and, generally this works well in your calculations so you shouldn't need to change this (you can if you like).

##### Set up your base-line-height. 
To prevent oddly spaced leading and large gaps between lines, we like to use a smaller measure, as you might do in print design. Something like 6px works well. Later, you'll use multiples of this baseline to creating your leading and spacing. 

#### In typography.scss

##### Setting your default headings, paragraphs, lists etc… using compass' rhythm and adjust-font-size-to commands. 
Whenever you adjust the font-size to using the adjust-font-size-to (e.g. 26px) this becomes your base unit to work out how many lines spacing you need above and below the element. e.g.

``` scss
@include adjust-font-size-to(26px); // Adjusts font size to 26px
	
margin: 0 0 rhythm(2, 26px) 0; // Adds two lines of our base-line-height (6x2 = 12px) below the element, base those two lines on our font size, and covert to ems. 
```

Have a play. It makes more sense when you do.

##### So:

``` scss
@include adjust-font-size-to(26px); 
margin: 0 0 rhythm(2, 26px) 0;    
```

##### Gives us:					

``` scss
font-size: 1.625em;
line-height: 1.15385em;
margin: 0 0 0.46154em 0;
```

#### Spacing blocks and other elements

If you want to vertically space other elements on the page (sections etc…) you can use the Compass leader and trailer functions.

``` scss
@include padding-leader(x); // adds x lines of padding, based on the base-line-height above the element. 
@include padding-trailer(x); // adds x lines of padding, based on the base-line-height below the element.
@include leader(x); // adds x lines of margin, based on the base-line-height above the element. 
@include trailer(x); // adds x lines of margin, based on the base-line-height below the element.
```

#### One final trick

Say - for example you'd like to add a pixel value to a media query, but you'd like to have that value convert to the relevant em value for the base-line-height or base-font-size. That might mean a few calculations. Sassaparilla includes two functions to help with this.

```
em-font(#px) // converts the value to pixels, based on the base-font-size.
	
em-base(#px) // converts the value to pixels, based on the base-line-height.
```

We also tend to like these for fine-tuning elements such as letter spacing. 


#### Using with Wordpress

Copy assets and config.rb into your theme folder.

#### Lastly

We've included #grid by Jon Gibbins and set it to generate from our baseline settings. This provides a nice visual check for baseline settings (although we should note each browser handles em maths slightly differently).

You can check out the [Hashgrid](http://hashgrid.com) website for instructions on how its used.

---

## Sassaparilla markup guidelines

At fffunction we write our css using the following rules. If you want to follow our lead, then the below should help.

- No ID's for styling
- Write in all lowercase and separate each word with a dash (.global-header)
- Use 4space tab indenting. Indent as you go
- Nest as little as possible to achieve the desired control - keeps the compiled CSS file size down
- Leave comments you wish to compile to css in regular css style. All other comments, write in SCSS style
- Use @extend and @mixin to keep code nice and lean

## The legal bit
This work is licensed under the Creative Commons Attribution-ShareAlike 3.0 Unported License. To view a copy of this license, visit [The Creative Commons Website](http://creativecommons.org/licenses/by-sa/3.0/).
