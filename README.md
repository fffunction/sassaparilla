# Sassaparilla

A quick start template for (hopefully) better websites.

## Overview

Sassaparilla is a set of default rules and styles that starts everything we do at [fffunction](http://fffunction.co) in a consistent manner. 

It’s not a boilerplate or a theme.

Sarsaparilla on the other hand, is a nice refreshing root beer. Perhaps crack one open while you’re reading this.

--- 

## Quick start

If you're ready to go, then simply delete the demo files and folders from assets and templates.

---

## Getting Sass(y) with Compass

Sassaparilla uses the power of [Sass](http://Sass-lang.com) via SCSS and [Compass](http://compass-style.org) to create flexible stylesheets that we can re-use and add to over time.

Compass is a library of common elements, equations and helper styles that is kept up-to-date by a community of developers and is useful for rapid development and leaner code.

Both Sass and Compass are Ruby Gems and will need to be installed via command line or terminal (if you’re on a mac you’re already rocking Ruby baby).

You can compile using tools such as [Mixture](http://mixture.io) or go hardcore and just use command line. Either way it’s pretty easy to get up and running.

You’ll find documentation on both the Sass and Compass websites on how to install and use them. If you plan on installing Compass (and you’ll need to), then you shouldn’t need to install Sass separately. 

To install compass on a Mac open Terminal and type:

### gem update --system 
This will update the system. Then

### gem install compass
To install compass

If you have trouble, try using the ‘sudo commands’ (with care) to access the correct level of permissions. E.g

### sudo gem update --system 
### sudo gem install compass

Of course, if you’re running Sassaparilla through [Mixture](http://mixture.io) you won’t need to do any of that.

--- 

## CSS set-up

Sassaparilla follows a specific cascade, and makes use of several SCSS files, each of which serves a different purpose.

Note: If a file begins with an underscore, that file will not compile to its own css file. Files with underscores are considered includes.

### screen.scss

This contains the bulk of your styles, as well as pulling together the other partial stylesheets into one css file. By and large you will be doing most of your work within this stylesheet.

### required.scss

This file contains referencing to the compass library element you will need. Hence it being required.

### reset.scss

This contains a simple reset for html elements. It assumes your webpage will have forms, so resets these too. If your site doesn’t use forms, you can remove these resets.

### settings.scss

This contains all the variables for the site. Things such as colours, break-points and font sizing live here. This file is for defining your core set-up and makes use of compass’ baseline and font-size measures. More on that later.

### mixins.scss

Mixins contains any user made mixins for the project. It includes some simple ones to get you started. Add more as needed.

### typography.scss

This file contains the core typesetting for the site. It relies on variables set-up in the settings.scss file. If you don’t wish to use compass’s rhythm method, you can leave this out.

### module-typeset.scss

This file contains typographic rhythm styles based on your variables. It allows for fine grain control of your typestyles by giving you classes of every element, to separate style from content. In addition, you can use or create more ‘typeset’ parent classes, which act as a way of setting text on particular areas of your website, where you need greater control. No more styling headings and lists globally and over-riding.

### forms.scss

Contains default form elements and standard styling. If you’re not using forms you can safely leave this out.

### grid.scss

Contains the main functionality of the Sassafras grid system. This file is where you add your columns and define your syntax for the surrounding ‘.rows’ in your content. We’ll touch on how to use this later.

--- 

## Modules and globbing

We've included a modules folder with an example `_navigation.scss` module. We like to chunk everything up into little bits in a BEM/SMACSS style, and this example structure is how we do so. 

We also use Sass globbing to pull all the files in from this folder with a `*` wildcard - if globbing is giving you issues, then you can add each module in maually in the base css file.

To install sass-globbing:

    $ gem install sass-globbing

--- 

## Working with breakpoints

We wanted to make things a little easier. 

We believe breakpoints should exist where design breaks. This means there maybe many or few, depending on your needs, and they should appear where you need them, not at device sizes. To do this, we’ve included three mixins and some variables, to allow you to configure things as you please.

##### Open up settings.scss

You’ll see our default breakpoints that we’ve used in the demo as a (somewhat extreme) example.  It looks a little like this:
	
	$a-vp								: 300px;	// first viewport 
	$b-vp								: 420px;	// up one
	$c-vp								: 600px;	// up two
	$d-vp								: 900px;	// up three
	$e-vp								: 1100px;	// up four
	$f-vp								: 1300px;	// up five
	$ie-fixed-vp						: 980px;    // Viewport for IE fixed width

	$max-vp 							: $f-vp; 	// max viewport. Syntactically makes for a nicer option
	$start-columns 						: $b-vp;	// Switch from single column to multi at this breakpoint (used in grid)

This set up allows you to define your syntax for your breakpoints as you wish. It also includes some variables that are required if using the Sassafras grid.

You’ll notice we’ve used alphabetical viewport names, to indicate where in the mobile first set-up they come. This way we can avoid using names such as tablet or mobile, which are too descriptive of a size.

##### Open up mixins.scss

Go to the section media queries and you’ll see three mixins which are similar. 

**respond-to**: Allows us to work mobile first and sets up a min-width only. 

**respond-to-max**: Allows us to define a maximum width breakpoint

**respond-to-min-max**: Allows us to set a range where the breakpoint is active.

	
To use simply include the mixin within a parent element in your code. For example
	
	.global-logo {
		color: #fff;
		@include respond-to($b-vp) {
			float: left;
		}
	}

Above we have told the mixin to show a minimum width of our second breakpoint ($b-vp) when the viewport reaches this width. 

That’s it. Have a play and see how you get on.

--- 

## Sassafras grid system

The grid system works a little differently than other grids you may have used in the past. That’s not always a bad thing, but as we always, use what is most appropriate for each project. Sassafras works best for simple grids layouts that require fine grain control at multiple breakpoints.

If you need an un-equal grid, multiple layer nesting or something more complex, then perhaps it’s not for you.

Sassafras has one main goal:

To allow for scalability and user-additions, that can be undertaken in an iterative manner. 

### How it works

Sassafras works by combining box-sizing:border-box and padding to space elements, and percentages to define column widths. As box-sizing:border-box considers padding as part of the box model, and not additional to it, we can use easy mathematics to generate our grid columns.

In short Sassafras can do two things.
+ Provide percentage with columns with no gutters, based on the amount of columns. You can then use internal padding to space elements apart.
+ Provide percentage with columns with gutters by adding an additional, consistent gutter padding and dragging a whole row negatively by the same consistent gutter padding.

It sounds more complex than it is.
Here’s a quick, step-by-step how to:

##### Open up mixins.scss

At the bottom of this file you’ll find the code for generating the grid columns.
	
	@mixin grid_columns($columns){
		@for $i from 1 through $columns{
			.colspan#{$columns}-#{$i}{
				width: ((100% / $columns)*$i);		
			}
		}
	}

You include this mixin like so, where # is the number of columns you need
	
	@include grid_columns(#); // Eg 12

This mixin generates percentages widths, based on the amount of columns we define in total (eg 12), and their individual spans as a multiple there of (eg 4 columns of 12). This looks a little like this when rendered.

	.colspan12-4 // Span four columns of a 12 column grid

This outputs 
	
	width: 33.3333333%; // 12 / 100 =  (8.3333333%  x 4 ) = 33.3333333%;

You can define as many column sets as you need. In addition, you’ll find mixins for push and pull columns.

	@for $i from 1 through $columns{
		.push#{$columns}-#{$i}{
			margin-left: ((100% / $columns)*$i);		
		}
	}
	@for $i from 1 through $columns{
		.pull#{$columns}-#{$i}{
			margin-right: ((100% / $columns)*$i);		
		}
	}
	
These work in the same manner as above
	
	.push12-4 // EG, push fours columns over in a 12 column grid

Underneath this in mixins.scss you’ll find

	@mixin grid {	
		// Widths
		*[class*="colspan"] {
			width: 100%; 
			display: block;
		}
		// Floats
		*[class*="as-grid"] {
			float: left;
			display: inline;
			display: inline-block;
		}
		// Gutters
		*[class*="with-gutter"] {
			@include respond-to($start-columns) {
				padding-left: $internal-spacing-percent; 
			}
		}
	}

This defines our css class hooks. For added flexibility, the grid is divided into three classes, which can be combined. These are:

	.colspan 

Targets all items that have a class that matches ‘colspan’. This gets the elements width (defaults to 100%) and nothing more. When we define as specific colspan, this is over-ridden (eg colspan12-4).
	
	.as-grid

Targets all items with a class of ‘as-grid’. Used in combination with ‘.colspan’ we take the width it defines as tell it to float, to form a grid item.
	
	.with-gutter

Targets all elements with a class of ‘with-gutter’. This tells the element to add a left-hand side gutter using padding. The ‘$start-columns’ variable is defined in settings.scss and informs the grid where it should start using gutters (as gutters might not be used at small sizes).

Using these elements might look something like
	
	colspan12-4							// Get the width but don’t float
	colspan12-4 as-grid 				// Get the width and float 
	colspan12-4 as-grid with-gutter		// Get the width, float and add gutter
	

##### Open up grid.scss

In this file you’ll add your columns to breakpoints within a parent element, to set-up a flexible grid system.

Using our respond-to mixin (as mentioned above) and our breakpoints for the settings file, we can build a mobile first grid system.

For example:
	
	@include respond-to($b-vp, $IE9: false) { 
		min-width: $b-vp;			// Min width of our second breakpoint
		@include grid_columns(2); 	// Two column grid
	}

This would output something like:

	@media screen and (min-width: 420px) {
		.row {
			min-width: 420px;
		}
		.row .colspan2-1 {
			width: 50%;
		}
		.row .colspan2-2 {
			width: 100%;
		}
	}

This gives us a two column grid, that starts at this breakpoint. From there on, it’s just a case of stacking more breakpoints as you go along, and including column numbers. 

**Note:** One limitation of this is that you cannot use the same amount of column numbers in multiple breakpoints. So if you use 12 at one breakpoint you cannot use it at another. 

In html, it should look something like this:

	<div class="row">
		
		<div class="colspan12-2 colspan6-2 colspan2-2  as-grid">
			<div class="as-grid__module">
				Content
			</div>
		</div>
		<div class="colspan12-2 colspan6-2 colspan2-2  as-grid">
			<div class="as-grid__module">
				Content
			</div>
		</div>
		<div class="colspan12-2 colspan6-2 colspan2-2  as-grid">
			<div class="as-grid__module">
				Content
			</div>
		</div>
		<div class="colspan12-2 colspan6-2 colspan2-2  as-grid">
			<div class="as-grid__module">
				Content
			</div>
		</div>
		<div class="colspan12-2 colspan6-2 colspan2-2  as-grid">
			<div class="as-grid__module">
				Content
			</div>
		</div>
		<div class="colspan12-2 colspan6-2 colspan2-2  as-grid">
			<div class="as-grid__module">
				Content
			</div>
		</div>
	</div>

###### Using gutters

If you wish to use gutters there are two more steps you must take

1. Create a parent element called ‘row-parent’.  This is usually only important if you wish to space stacked grid rows vertically in a consistent manner.
2. Add your ‘row’ as normal.
2. Add a class of ‘row__colspaced’ child row element. This tells the row to allow for guttered columns. You do not have to use the class of ‘row’ as these elements are nested inside a ‘row’ already.

It should looks something like this:
	
	<div class="row-parent">
		<div class="row">
			<div class="row__colspaced">
				<div class="colspan12-2 colspan6-2 colspan2-2  as-grid with-gutter">
					<div class="as-grid__module">
						Content
					</div>
				</div>
				<div class="colspan12-2 colspan6-2 colspan2-2  as-grid with-gutter">
					<div class="as-grid__module">
						Content
					</div>
				</div>
				<div class="colspan12-2 colspan6-2 colspan2-2 as-grid with-gutter">
					<div class="as-grid__module">
						Content
					</div>
				</div>
			</div>
			
			<div class="row__colspaced">
				<div class="colspan12-2 colspan6-2 colspan2-2  as-grid with-gutter">
					<div class="as-grid__module">
						Content
					</div>
				</div>
				<div class="colspan12-2 colspan6-2 colspan2-2  as-grid with-gutter">
					<div class="as-grid__module">
						Content
					</div>
				</div>
				<div class="colspan12-2 colspan6-2 colspan2-2 as-grid with-gutter">
					<div class="as-grid__module">
						Content
					</div>
				</div>
			</div>
			
		</div>
	</div>

There’s loads more you can do with the grid, and you can bend it to your will, and to use your syntax by just changing a few variables in the mixins.scss and settings.scss files.

Have a play.

---

## Compass rhythm and leading

Sassaparilla tries to make leading and spacing as easy as possible, whilst writing accessible code. We’re not mad keen on tricky maths, but we like to write in ems. Luckily, compass can handle this for us with a few smart defaults, tricks and a few commands. Here’s how:

#### In settings.scss

##### Setting up your base-font-size. 
This defaults to 16px and, generally this works well in your calculations so you shouldn’t need to change this (you can if you like).

##### Set up your base-line-height. 
To prevent oddly spaced leading and large gaps between lines, we like to use a smaller measure, as you might do in print design. Something like 6px works well. Later, you’ll use multiples of this baseline to creating your leading and spacing. 

#### In module-typeset.scss or typography.scss

##### Setting your default headings, paragraphs, lists etc… using compass’ rhythm and adjust-font-size-to commands. 
Whenever you adjust the font-size to using the adjust-font-size-to (e.g. 26px) this becomes your base unit to work out how many lines spacing you need above and below the element. e.g.

	@include adjust-font-size-to(26px); // Adjusts font size to 26px
	
	margin: 0 0 rhythm(2, 26px) 0; // Adds two lines of our base-line-height (6x2 = 12px) below the element, base those two lines on our font size, and covert to ems. 

Have a play. It makes more sense when you do.

##### So:

	@include adjust-font-size-to(26px); 
	margin: 0 0 rhythm(2, 26px) 0;    

##### Gives us:					
	font-size: 1.625em;
	line-height: 1.15385em;
	margin: 0 0 0.46154em 0;

#### Spacing blocks and other elements

If you want to vertically space other elements on the page (sections etc…) you can use the Compass leader and trailer functions.

	@include padding-leader(x); // adds x lines of padding, based on the base-line-height above the element. 
	@include padding-trailer(x); // adds x lines of padding, based on the base-line-height below the element.
	@include leader(x); // adds x lines of margin, based on the base-line-height above the element. 
	@include trailer(x); // adds x lines of margin, based on the base-line-height below the element.

#### One final trick

Say – for example you’d like to add a pixel value to a media query, but you’d like to have that value convert to the relevant em value for the base-line-height or base-font-size. That might mean a few calculations. Sassaparilla includes two functions to help with this.

	em-font(#px) // converts the value to pixels, based on the base-font-size.
	
	em-base(#px) // converts the value to pixels, based on the base-line-height.

We also tend to like these for fine-tuning elements such as letter spacing. 


#### Using with Wordpress

Copy assets and config.rb into your theme folder.

---

## Sassaparilla markup guidelines

At fffunction we write our css using the following rules. If you want to follow our lead, then the below should help.

- No ID’s for styling
- Write in all lowercase and separate each word with a dash (.global-header)
- Use 4space tab indenting. Indent as you go
- Nest as little as possible to achieve the desired control - keeps the compiled CSS file size down
- Leave comments you wish to compile to css in regular css style. All other comments, write in SCSS style
- Use @extend and @mixin to keep code nice and lean

## The legal bit
Use how you want. We’d love to see how it’s being used, so do tell. 

Please don’t sell Sassparilla wholesale (as is), as that’s just not very nice. If we find out this is the case, we’ll send around a big smelly dog to sit on all your soft furnishings. You have been warned.
