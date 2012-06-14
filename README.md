# Sass & Compass: Never Write Regular CSS Again

## What are they
* Ruby gems
* CSS preprocessors
* Generates normal CSS files

### How do they work
* "Watch" a file/folder
* If anything changes in the file/folder, regenerates new CSS

## How to install
* gem install sass
* gem install compass

## How to get started
* Almost no reason to use Sass and not Compass. Compass adds additional functionality without any downside.
* Rails project vs standalone

### For a standalone project
* compass create <myproject> OR
* create a config.rb file manually with the necessary paths for your project

		http_path = "/"
		css_dir = "stylesheets/"
		sass_dir = "compile/sass"

* create a sass or scss file in your sass_dir.
* compass watch on the command line
* add CSS to this file, and it will auto generate a CSS file in your css_dir



## Sass Syntax
* SASS vs SCSS: I much prefer SCSS syntax.

> Sass has two syntaxes. The new main syntax (as of Sass 3) is known as “SCSS” (for “Sassy CSS”), and is a superset of CSS3’s syntax. This means that every valid CSS3 stylesheet is valid SCSS as well. SCSS files use the extension .scss.

> The second, older syntax is known as the indented syntax (or just “Sass”). Inspired by Haml’s terseness, it’s intended for people who prefer conciseness over similarity to CSS. Instead of brackets and semicolons, it uses the indentation of lines to specify blocks. Although no longer the primary syntax, the indented syntax will continue to be supported. Files in the indented syntax use the extension .sass.


### Nesting
I used to hate nesting, but I have learned to love it. It's not something you HAVE to use. Be careful that you don't go too far because the code gets sloppy and hard to read.

	/* scss */
	.nav {
		background: #000;

		li {
			float: left;
		}

		a {
			color: #fff;
		}
	}

	/* css */
	.nav {
		background: #000;
	}
	.nav li {
		float: left;
	}
	.nav a {
		color: #fff;
	}


Example with too many selectors

	/* scss */
	.nav {
		ul {
			li {
				a {
					span {
						color: #fff
					}
				}
			}
		}
	}
	
	/* css */
	.nav ul li a span {
		color: white;
	}



### Parent references
The & becomes the selector

	/* scss */
	a {
		color: red;
	
		&:hover {
			color: blue;
			text-decoration: underline;
		}
	}

	/* css */
	a {
		color: red;
	}
	a:hover {
		color: blue;
		text-decoration: underline;
	}


	/* scss */
	img {
		display: block;
	
		.lte7 & {
			-ms-interpolation-mode: bicubic;
		}
	}

	/* css */
	img {
		display: block;
	}
	.lte7 img {
		-ms-interpolation-mode: bicubic;
	}




### Variables
Variables are exactly what you have always wanted out of variables in CSS

	/* scss */
	$linkColor: #ce4dd6;

	a {
		color: $linkColor;
	}

	/* css */
	a {
		color: #ce4dd6;
	}

You can use it in the property and selector too (interpolation)

	/* scss */
	$side: top;
	
	.bordered-#{$side} {
		border-#{$side}: 1px solid green;
	}
	
	/* css */
	.bordered-top {
		border-top: 1px solid green;
	}



### Operations
The standard math operations (+, -, *, /, and %) are supported for numbers, even those with units. For colors, there are all sorts of useful functions for changing the lightness, hue, saturation, and more.

	/* scss */
	$col: 100px;

	.main {
		width: $col * 6;
	}
	
	/* css */
	.main {
		width: 600px;
	}
	
	
	/* scss */
	.box {
		background: rgba(#000, 0.5);
	}
	
	/* css */
	.box {
		background: rgba(0, 0, 0, 0.5);
	}
	
	
	/* scss */
	a {
		color: #000;

		&:hover {
			color: lighten(#000, 0.75);
		}
	}
	
	/* css */
	a {
		color: #000;
	}

	a:hover {
		color: #020202;
	}



### Mixins
Reuse styles without having to duplicate. Make your code DRY.

	/* scss */
	@mixin box-highlight() {
		background: green;
		border: 1px solid red;
		padding: 20px;
		width: 100%;
	}

	.call-out {
		@include box-highlight();
		color: yellow;
	}

	.special-box {
		@include box-highlight();
	}
	
	/* css */
	.call-out {
		background: green;
		border: 1px solid red;
		padding: 20px;
		width: 100%;
		color: yellow;
	}

	.special-box {
		background: green;
		border: 1px solid red;
		padding: 20px;
		width: 100%;
	}
	
You can also pass parameters
	
	/* scss */
	@mixin section($background: #000, $color: #fff, $radius: 10px) {
		background: $background;
		border-radius: $radius;
		color: $color;
		padding: 20px;
	}

	.section-1 {
		@include section();
	}

	.section-2 {
		@include section(#cecece, #000, 5px);
	}
	
	/* css */
	.section-1 {
		background: black;
		border-radius: 10px;
		color: white;
		padding: 20px;
	}

	.section-2 {
		background: #cecece;
		border-radius: 5px;
		color: black;
		padding: 20px;
	}

You can even include child selectors

	/* scss */
	@mixin shadowed() {
		overflow: hidden;
		position: relative;

		h2 {
			font-size: 36px;
		}

		&:before {
			box-shadow: 0 0 10px rgba(#000, 0.5);
			content: "";
			height: 20px;
			left: 10px;
			position: absolute;
			right: 10px;
			top: -20px;
		}
	}

	.section {
		@include shadowed();
	}
	
	/* css */
	.section {
		overflow: hidden;
		position: relative;
	}

	.section h2 {
		font-size: 36px;
	}

	.section:before {
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
		content: "";
		height: 20px;
		left: 10px;
		position: absolute;
		right: 10px;
		top: -20px;
	}


In newer versions of Sass, you can do content block mixins

	/* scss */
	@mixin respond-to($media) {
		@if $media ==  small-and-under {
			@media only screen and (max-width: 479px) { @content; }
		}
		@else if $media == medium {
			@media only screen and (min-width: 480px) and (max-width: 767px) { @content; }
		}
		@else if $media == full {
			@media only screen and (min-width: 1000px) { @content; }
		}
	}

	.some-thing {
		width: 800px;

		@include respond-to(medium) {
			width: 100%;
		}
	}
	
	/* css */
	.some-thing {
		width: 800px;
	}
	@media only screen and (min-width: 480px) and (max-width: 767px) {
		.some-thing {
	  		width: 100%;
		}
	}


### Extend
Kinda like mixins, but kinda not. Sort of a replacement for mixins with no parameters. Extends only work for single elements. Can't do "@extend .nav a"
	
	/* scss */
	.box-highlight {
		background: green;
		border: 1px solid red;
		padding: 20px;
		width: 100%;
	}

	.hightlight {
		@extend .box-highlight;
		color: yellow;
	}

	.another-highlight {
		@extend .box-highlight;
	}
	
	/* css */
	.box-highlight, .highlight, .another-highlight {
		background: green;
		border: 1px solid red;
		padding: 20px;
		width: 100%;
	}

	.highlight {
		color: yellow;
	}

In newer versions of Sass, you can also use "silent" extends so the initial base class doesn't end up in your CSS.

	/* scss */
	%font-georgia {
		color: #666;
		font-family: Georgia;
		font-style: italic;
	}

	.heading {
		@extend %font-georgia;
	}

	.footnote {
		@extend %font-georgia;
	}
	
	/* css */
	.heading, .footnote {
		color: #666;
		font-family: Georgia;
		font-style: italic;
	}



### Loops
For loop

	/* scss */
	$col: 100px;
	$gutter: 20px;

	@for $i from 1 through 5 {
		.span-#{$i} {
			width: ($i * $col) + ($gutter * ($i - 1));
		}
	}
	
	/* css */
	.span-1 {
		width: 100px;
	}

	.span-2 {
		width: 220px;
	}

	.span-3 {
		width: 340px;
	}

	.span-4 {
		width: 460px;
	}

	.span-5 {
		width: 580px;
	}


Each loop

	/* scss */
	@each $service in twitter, facebook, linkedin, flickr {
		.#{$service} {
			background-image: url(/images/icons/#{$service}.png);
		}
	}

	/* css */
	.twitter {
		background-image: url(/images/icons/twitter.png);
	}

	.facebook {
		background-image: url(/images/icons/facebook.png);
	}

	.linkedin {
		background-image: url(/images/icons/linkedin.png);
	}

	.flickr {
		background-image: url(/images/icons/flickr.png);
	}



### Import
You can break your CSS into multiple files, and Sass will combine into a single file so you don't take a hit from extra HTTP requests.
	
	/* scss */
	@import "forms";
	@import "buttons";
	
	/* css */
	input {
		background: #dedede;
		border: 1px solid #000;
	}

	button {
		background: #000;
		color: #fff;
		padding: 5px 10px;
		text-transform: uppercase;
	}


## Compass Features
* Compass basically gives you a whole bunch of prebuilt mixins to use.
* Import all of the CSS3 mixins

		@import "compass/css3";

* Then you can use those mixins just like you would any other mixin

### Built-in mixins

	/* scss */
	.rounded-box {
		@include border-radius(10px);
	}
	
	/* css */
	.rounded-box {
		-webkit-border-radius: 10px;
		-moz-border-radius: 10px;
		-ms-border-radius: 10px;
		-o-border-radius: 10px;
		border-radius: 10px;
	}
	
Gradients become sooooo simplified

	/* scss */
	.gradient {
		@include background-image(linear-gradient(#fff, #ccc));
	}
	
	/* css */
	.gradient {
		background-image: -webkit-gradient(linear, 50% 0%, 50% 100%, color-stop(0%, #ffffff), color-stop(100%, #cccccc));
		background-image: -webkit-linear-gradient(#ffffff, #cccccc);
		background-image: -moz-linear-gradient(#ffffff, #cccccc);
		background-image: -o-linear-gradient(#ffffff, #cccccc);
		background-image: -ms-linear-gradient(#ffffff, #cccccc);
		background-image: linear-gradient(#ffffff, #cccccc);
	}
	
Tons more: http://compass-style.org/reference/compass/css3/


### Spriting
Compass can automatically combine images and create sprites. It's pretty amazing.

	/* scss */
	$icons-spacing: 20px;
	@import "icons/*.png";

	@each $service in twitter, facebook, linkedin, flickr {
		.#{$service} {
			@include icons-sprite(#{$service});
		}
	}
	
	/* css */
	.icons-sprite, .twitter, .facebook, .linkedin, .flickr {
		background: url('/images/icons-s589c2b097f.png') no-repeat;
	}

	.twitter {
		background-position: 0 -75px;
	}

	.facebook {
		background-position: 0 -223px;
	}

	.linkedin {
		background-position: 0 0;
	}

	.flickr {
		background-position: 0 -149px;
	}

If you need to adjust the background-position

	/* scss */
	@each $service in twitter, facebook, linkedin, flickr {
		.#{$service} {
			@include icons-sprite(#{$service}, $offset-x: 10px, $offset-y: 10px);
		}
	}
	
	/* css */
	.twitter {
		background-position: 10px -65px;
	}

	.facebook {
		background-position: 10px -213px;
	}

	.linkedin {
		background-position: 10px 10px;
	}

	.flickr {
		background-position: 10px -139px;
	}