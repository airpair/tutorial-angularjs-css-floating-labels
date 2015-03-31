##About this tutorial

We will be styling the html input fields to have floating labels along with the validations of angularJS. A working example of the same can be found [here](http://gunua-works.com/air-pair/floating-labels/example/).

Well, by working I mean in 'terms of styles' of the floating label. It doesn't actually submit.

####Why is this tutorial is so lengthly when it explains such a simple thing?
Even if you don't have this question, I still want to answer it. When I was new to angular (and bower, npm ), I used to see many examples, I would call them 'broken' examples as they did tell something good but they did not contain every other detailed information which a beginner would need. Hence, I am going to explain everything in detail here in a step-by-step way, following which *anyone* would be able to create this HTML form. Having extra information does not hurt,  if you are too familiar with most of the things explained here, you can just skip through and look at the SCSS code which would be of interest to you.  

You can also see the source code of this example at this [Github repository](https://github.com/sambhav-gore/material-input-fields)


During this course we will also be briefly touching the topics of:
- How to use Bower 
- Using SCSS and 'sass --watch' for your .scss files
- A few nice CSS techniques like _::after_ pseudo selectors and transitions


_Note - There may be several ways to make this same example, also you can use an existing framework like [angular-material](https://material.angularjs.org/#/) to achieve similar results. But this tutorial is about how to make the same functionality using hand-coded CSS (or SCSS which we will be using)_

### Prerequisites

In order to go through this you will need to know basics of :

- [Sass](http://sass-lang.com/) 
- [AngularJS](https://angularjs.org/)
- [Bower](http://bower.io/)

You will need to have the following installed and configured in your development machine:

- [Node and NPM](https://nodejs.org/)
- [Bower](http://bower.io/)
- [Sass](http://sass-lang.com/install) 


_This tutorial assumes you are using Mac or Linux and executing all the commands in Terminal. If you are on Windows, I highly recommend to either switch to Ubuntu (or any other flavor of Linux) OR use bash instead of command prompt_

## Setting up the Demo Project

First, Create a new folder, and inside the folder create a file named `index.html`.
For the sake of this tutorial we are going to call this folder `app-root`. 

Next, Create a few more folders so that your directory structure looks like the following:

```
app-root
    /app
    /assets
        /css
        /scss
    /index.html
```

Next, Open terminal and cd into your `app-root` folder.
We will use Bower to get our angularJS dependency

```shell
cd ~/path/to/app-root
bower install angular
```

Bower will install angular js in a folder 'bower-components' within your app-root folder.

After this, we will create a very basic angularJS app for our demo.

Inside your app-root/app folder create a new file called `app.js`

Create a very basic app module in the app.js file. Open the app.js file in your text editor and type the following:

```js
'use strict'

angular.module("app",[]);
```

That's it, we are not going to use any other Angular modules or dependencies.


Next, we need to create an SCSS file for us where we are going to write our SCSS code. So create a new file named `styles.scss` under app-root/assets/scss folder.


Now your folder should be looking like the following:
```
app-root
    /app
        /app.js
    /assets
        /css
        /scss
            /styles.scss
    /bower_components
    /index.html
```

###Writing our HTML Markup.
Open the index.html file in your favorite text-editor. (I use sublime but you can use pretty much any text-editor you like).

Following is the basic HTML we will be using in this example

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Material Input Fields</title>
</head>
<body ng-app="app">
	<div class="wrapper">
		<div class="form-wrapper">
			<form name="form" novalidate>
				<div class="form-group">
				    <label for="name"></label>
					<input type="text" name="name" ng-model="user.name">
					<p class="error-block">Name is Required!</p>
				</div>
			</form>
		</div>
	</div>
</body>
</html>
```

We need to add angular js to this. 
A line before the ending `body` tag, insert this line :
```html
	<script src="bower_components/angular/angular.min.js"></script>
```

Also import our app.js file. 
After the angular.min.js import, insert the following line 
```html
	<script src="app/app.js"></script>
```

###Watching SCSS file to generate the CSS file
In this tutorial we are going to use `sass` for watching our scss file and converting it to a css file.


Before we start watching our SCSS file for changes, we will define the very basic styles in it.
So open the app-root/assets/scss/styles.scss file in your text editor and insert the following styles
```scss
body {
	margin:0;
	padding: 10px;
	background: #f9f9f9;
	font-family: helvetica, arial, sans-serif; 
	box-sizing: border-box;
}

.wrapper {
	width: 100%;
}

.form-wrapper {
	width: 340px;
	margin: 60px auto;
	padding: 24px;
	background: #fff;
	box-shadow: 2px 2px 2px rgba(0,0,0,0.1);
}
```


In order to watch your scss file, you need to have `sass` installed on your machine. If you do not have sass please visit http://sass-lang.com/install 
 
Open a new terminal window
```ssh
cd /path/to/app-root/assets/
sass --watch scss:css
```
This will generate styles.css in your assets/css/ folder. If sass is working well then it will also keep watching for any changes in the SCSS file and automatically update the CSS file.

Now, we need to import the newly generated CSS file in our demo app, so go back to your index.html file in the text editor and import the css file in the `head` section, just below the `title`

```html
	<link rel="stylesheet" href="assets/css/styles.css">
```

_pro tip : Styles go in head while scripts go at bottom in your html_

As of now, your HTML should be having the following code

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Material Input Fields</title>

	<link rel="stylesheet" href="assets/css/styles.css">
</head>
<body ng-app="app">
	<div class="wrapper">
		<div class="form-wrapper">
			<form name="form" novalidate>
				<div class="form-group">
				    <label for="name">Enter a Name</label>
					<input type="text" name="name" ng-model="user.name" required>
					<p class="error-block">Name is Required!</p>
				</div>
			</form>
		</div>
	</div>
	{{user.name}}
	<script src="bower_components/angular/angular.min.js"></script>
	<script src="app/app.js"></script>
</body>
</html>
```
##Defining ng-class conditions and our CSS selectors
For the floating label to work correctly, we need to define conditional CSS selectors using `ng-class` directive for the following states:

- When the field is empty and does not have focus
- When the field has focus
- When the field is not empty and does not have focus
- When field has an error

In order to do this correctly, we will define these conditions on the `form-group` div


Edit the html file and change the form-group div to include the following ng-class conditions

```
ng-class='{ "has-focus": form.name.hasFocus,
			"has-success": form.name.$valid,
			"has-error": form.name.$invalid && (form.$submitted || form.name.$touched),
			"is-empty": !form.name.$viewValue }'
```

Above mentioned conditions are self explanatory, we want the group div to have class of `has-success` when the field is valid, `has-error` when the field is not valid and has been touched by the user, `is-empty` when the field is empty, And, `has-focus` when the field is having focus. 

The 'hasFocus' property that we are checking against, we need to set it to true when the field does have focus. So the following two `ng-focus` and `ng-blur` directives need to be added to the input field itself

```
ng-focus='form.name.hasFocus=true', ng-blur='form.name.hasFocus=false'
```

lastly, we need to show the error message only when:
- The field is invalid; &
- The field is touched by user OR the user has attempted to submit the form without filling the field.

For this purpose, the following `ng-show` condition needs to be added to the `error-block` p tag.

```
ng-show="form.name.$error.required && (form.name.$touched || submitted)"
```

After writing this much your HTML should be looking like the following:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Material Input Fields</title>

	<link rel="stylesheet" href="assets/css/styles.css">
</head>
<body ng-app="app">
	<div class="wrapper">
		<div class="form-wrapper">
			<form name="form" novalidate>
				<div class="form-group" 
						ng-class='{ "has-focus": form.name.hasFocus,
						"has-success": form.name.$valid,
						"has-error": form.name.$invalid && (form.$submitted || form.name.$touched),
						"is-empty": !form.name.$viewValue }'>
					<label for="name">Enter a Name</label>
					<input type="text" name="name" ng-model="user.name" required
						ng-focus='form.name.hasFocus=true', ng-blur='form.name.hasFocus=false'>
					<p ng-show="form.name.$error.required && (form.name.$touched || submitted)"  class="error-block">Name is Required!</p>
				</div>
			</form>
		</div>
	</div>

	<script src="bower_components/angular/angular.min.js"></script>
	<script src="app/app.js"></script>
</body>
</html>
```

If you test at this point of time in browser, you can see in the elements inspector that the classes which we need do get added to the field correctly. And also our errror is shown only when it needs to be shown.

Now we move on to the next part, which is writing the SCSS code to `materialize` it all!

## Writing the CSS rules (in the .scss file)

Open the styles.scss in your text editor.

First, We need to declare all these selectors which we would need. Once again, if you do not know Sass, please head over to http://sass-lang.com to understand the basics of it. 

_The compiled css of finished example is available at the bottom of this post_

```scss
.form-group {
	/*styles for the form-group itself*/
	
	
	input[type='text'] {
		/*Default styles for field*/	
	}

	label {
		/*Default styles for label*/
	}

	&::after {
		/*Default styles for 'after' pseudo selector for form-group*/
	}

	&.has-focus {
		/*When field has focus*/
	}

	&.has-error{
		/*When field has error*/

	}

	&.has-success{
		/*When field has success*/

	}
	
	&.is-empty {
		/*When Field is empty*/
	}

}
```

First we will declare the styles for default states for each starting with form group itself

```scss
/*styles for the form-group itself*/
	position: relative;
	width: 100%;
	margin-bottom: 42px;
	
	input[type='text'] {
        /*Default styles for field*/	
        position: relative;
        width: 100%;
        background: 0 0;
        padding: 26px 0 12px;
        font-size: 15px;
        line-height: 1.4;
        font-weight: 500;
        border: 1px solid rgba(0,0,0,.08);
        border-width: 0 0 1px;
        box-shadow: none;
        z-index: 1;

	}

	label {
		/*Default styles for label*/
		display: inline-block;
		position: absolute;
		margin-bottom: 6px;

		font-size: 12px;
  	    font-weight: 300;

		color: rgba(0,0,0,0.4);
		padding: 0;
		transition: all .3s ease;
	}
```

Next, we will add the default styles for the ::after pseudo element. We basically use this ::after element to show the animation below the field when the field recieves focus or when it has error. 

Idea is that first we define the default (resting state) styles for this ::after pseudo element, We will also define a css transition in this. Afterwards we will change the property values of this pseudo element so the border (bottom) of the field will 'appear' to be animating. But actually it is not the border which animates, it is the this pseudo element. 
`note - the input is having z-index lower than this pseudo element`

```scss
&::after {
		/*Default styles for 'after' pseudo selector for form-group*/
		content: "";
	  position: absolute;
	  width: 0;
	  height: 2px;
	  bottom: 0;
	  left: 50%;
	  z-index: 1000;
	  background: rgba(0,0,0,.08);
	  transition: all .3s ease;
	}
```

At this time, your SCSS file should be looking like the following:

```scss

body {
	margin:0;
	padding: 10px;
	background: #f9f9f9;
	font-family: helvetica, arial, sans-serif; 
	box-sizing: border-box;
}

.wrapper {
	width: 100%;
}

.form-wrapper {
	width: 340px;
	margin: 60px auto;
	padding: 24px;
	background: #fff;
	box-shadow: 2px 2px 2px rgba(0,0,0,0.1);
}

.form-group {
	/*styles for the form-group itself*/
	position: relative;
	width: 100%;
	margin-bottom: 42px;
	
	input[type='text'] {
        /*Default styles for field*/	
        position: relative;
        width: 100%;
        background: 0 0;
        padding: 26px 0 12px;
        font-size: 15px;
        line-height: 1.4;
        font-weight: 500;
        border: 1px solid rgba(0,0,0,.08);
        border-width: 0 0 1px;
        box-shadow: none;
        z-index: 1;

	}

	label {
		/*Default styles for label*/
		display: inline-block;
		position: absolute;
		margin-bottom: 6px;
		top:0;

		font-size: 12px;
  	    font-weight: 300;

		color: rgba(0,0,0,0.4);
		padding: 0;
		transition: all .3s ease;
	}

	&::after {
		/*Default styles for 'after' pseudo selector for form-group*/
        content: "";
        position: absolute;
        width: 0;
        height: 2px;
        bottom: 0;
        left: 50%;
        z-index: 1000;
        background: rgba(0,0,0,.08);
        transition: all .3s ease;
	}

	&.has-focus {
		/*When field has focus*/
	}

	&.has-error{
		/*When field has error*/

	}

	&.has-success{
		/*When field has success*/

	}
	
	&.is-empty {
		/*When Field is empty*/
	}

}
```

### Writing the rest of the CSS rules in your SCSS file.

Now, we will first concentrate on 'when field has focus'. We need to perform the following actions when the field gets focused:

1. Move the label Up
2. Animate the border to reveal 'focus' color

In order 'move' the label up, we need to have the label at the correct position, which it is not at the moment. If you run the file right now, you will notice that the label is already 'up'.
So first, we need to bring the label down and let it occupy the position where the text input is.

Note, we need to _bring down_ the label _Only when_ the field is empty.

If you go back and see the `ng-class` conditions above, you will see that we are adding `is-empty` class to the form-group div when the field is empty. We can make use of that.
Another important thing is that if the field gets focus then this label will be moved. So our css rule should only target the condition where - 
1. The field _is_ empty ; and;
2. The field is not having focus. We will modify our `.is-empty` nested selector for this within the nested `.form-group` (Last selector in the previous step).

```scss
.form-group {
    .
    . /*Other styles*/
    .
    &.is-empty:not(.has-focus) label {
		font-weight: 400;
		font-size: 14px;
		top:32px;
	}
}
```

If you test your HTML at this point, you will see that our floating Label is already done. 

Now we need change the label color when focused and also add the bottom animating border for the field. As explained above, we will be animating the `::after` pseudo element for this.

In the previously defined empty block of `&.has-focus` add the following:

```scss
.form-group {
    .
    . /*Other styles*/
    .
    
    &.has-focus {
		/*When field has focus*/
		label {
			color: #039BE5;
		}

		&::after {
			left:0;
			background: #039BE5;
			width: 100%;
			/*We are changing the width of the pseudo element from 0 to 100%. As there is already transition added to it, the change in width will 'animate'. As we have strategically positioned this pseudo element just below the input with a 2px height, it will 'appear' as bottom-border for the input (which it is actually not)*/
		}			
	}
	
	.
	./*other styles*/
	.
}
```

If you run HTML right now, you will see the border works correctly. But if the field goes in error state, then the border comes at a wrong place.
For this purpose, we need to make the `error-block` as absolutely positioned. Also, instead of hiding using `display:none;` we will use the 'opacity' and 'visibility' properties so that we can animate that error message too.

For this, we need to add two code blocks, one directly under the `form-group` nested selector, and the other one when form-group has error (we need to display the error when form-group has a class of 'has-error')

```scss
.form-group {
    .
    . /*Other styles*/
    .
    
    .error-block {
		/*Default Styles for Help-block*/
		position: absolute;
		line-height: 0;
		font-weight: 300;
		font-size: 12px;
		color: red;
		line-height: 24px;
		transition: all 0.3s ease;
		margin:0;

		&.ng-hide {
			display: block !important;
			visibility: hidden;
			opacity: 0;
			top: calc(100% - 24px);
		}
	}
	
	.
	. /*Other existing styles*/
	.
	
	&.has-error {
	    .
	    . /*Other existing styles*/
	    .
	    
	    .help-block:not(.ng-hide) {
		    position: absolute;
		    color: red;
		    opacity: 1;
		    visibility: visible;
		    top:100%;
		}
	}
    
	.
	. /*Other existing styles*/
	.
	
}
```


Similarly, we will go ahead and define our remaining styles, which are `has-error` and `has-success`.

change the `has-error` block we defined to the following:
```scss
.form-group {
    .
    . /*Other styles*/
    .
    
    &.has-error{
		/*When field has error*/
		label {
			color: red;
		}

			
		&::after {
			left: 0;
			background: red;
			width: 100%;
		}

        /*We already added this :not selector in the previous step. I am keeping it here to avoid confusion.*/
		.help-block:not(.ng-hide) {
		    position: absolute;
		    color: red;
		    opacity: 1;
		    visibility: visible;
		    top:100%;
		}

	}
	
	.
	.
	.
}
```

Change the `has-success` block we defined to the following:

```scss
.form-group {
    .
    . /*Other styles*/
    .
    
    &.has-success{
		/*When field has success*/
		label {
			color: forestgreen;
		}

		&.has-focus::after {
			left:0;
			background: forestgreen;
			width: 100%;
		}
	}
	
	.
	.
	
}
```

All good so far, If you test right now, everything should be working correctly. Well, Almost... The only issue remaining is that when the field is having valid input and we `focus-out` of the field, it is still retaining the 'green' color for label. To solve this issue, We will implement our `has-suceess` styles _only_ when the field has focus. So, Just add `.has-focus` in the `.has-success` selector. Hence, final success selector style would be as follows


```scss
.form-group {
    .
    . /*Other styles*/
    .
    
    &.has-success.has-focus{
		/*When field has success*/
		label {
			color: forestgreen;
		}

		&.has-focus::after {
			left:0;
			background: forestgreen;
			width: 100%;
		}
	}
	.
	.
	.
}
```

And thats all! Test it in the browser and you will see everything should be working just fine.

##Here are our final source files :

###SCSS

```scss
body {
	margin:0;
	padding: 10px;
	background: #f9f9f9;
	font-family: helvetica, arial, sans-serif; 
	box-sizing: border-box;
}

.wrapper {
	width: 100%;
}

.form-wrapper {
	width: 340px;
	margin: 60px auto;
	padding: 24px;
	background: #fff;
	box-shadow: 2px 2px 2px rgba(0,0,0,0.1);
}

.form-group {
	/*styles for the form-group itself*/
	position: relative;
	width: 100%;
	margin-bottom: 42px;
	
	input[type='text'] {
		/*Default styles for field*/	
	  position: relative;
		width: 100%;
	  background: 0 0;
	  padding: 26px 0 12px;
  	font-size: 15px;
  	line-height: 1.4;
  	font-weight: 500;
	  border: 1px solid rgba(0,0,0,.08);
	  border-width: 0 0 1px;
	  box-shadow: none;
	  z-index: 1;

	  &:focus {
	  	outline: 0;
	  }
	}

	label {
		/*Default styles for label*/
		display: inline-block;
		position: absolute;
		margin-bottom: 6px;
		top:0;

		font-size: 12px;
  	font-weight: 300;

		color: rgba(0,0,0,0.4);
		padding: 0;
		transition: all .3s ease;

	}

	&::after {
		/*Default styles for 'after' pseudo selector for form-group*/
		content: "";
	  position: absolute;
	  width: 0;
	  height: 2px;
	  bottom: 0;
	  left: 50%;
	  z-index: 1000;
	  background: rgba(0,0,0,.08);
	  transition: all .3s ease;
	}

	.error-block {
		/*Default Styles for Help-block*/
		position: absolute;
		line-height: 0;
		font-weight: 300;
		font-size: 12px;
		color: red;
		line-height: 24px;
		transition: all 0.5s ease;
		margin:0;

		&.ng-hide {
			display: block !important;
			visibility: hidden;
			opacity: 0;
		}
	}

	

	&.has-focus {
		/*When field has focus*/
		label {
			color: #039BE5;
		}

		&::after {
			left:0;
			background: #039BE5;
			width: 100%;
			/*We are changing the width of the pseudo element from 0 to 100%. As there is already transition added to it, the change in width will 'animate'. As we have strategically positioned this pseudo element just below the input with a 2px height, it will 'appear' as bottom-border for the input (which it is actually not)*/
		}			
	}

	&.has-error{
		/*When field has error*/
		label {
			color: red;
		}

			
		&::after {
			left: 0;
			background: red;
			width: 100%;
		}

		.help-block:not(.ng-hide) {
		    position: absolute;
		    color: red;
		    opacity: 1;
		    visibility: visible;
		    top:100%;
		}

	}

	&.has-success.has-focus{
		/*When field has success*/
		label {
			color: forestgreen;
		}

		&.has-focus::after {
			left:0;
			background: forestgreen;
			width: 100%;
		}
	}

	
	&.is-empty:not(.has-focus) label {
		font-weight: 400;
		font-size: 14px;
		top:32px;
	}
}
```

###HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Material Input Fields</title>

	<link rel="stylesheet" href="assets/css/styles.css">
</head>
<body ng-app="app">
	<div class="wrapper">
		<div class="form-wrapper">
			<form name="form" novalidate>
				<div class="form-group" 
						ng-class='{ "has-focus": form.name.hasFocus,
						"has-success": form.name.$valid,
						"has-error": form.name.$invalid && (form.$submitted || form.name.$touched),
						"is-empty": !form.name.$viewValue }'>
					<label for="name">Enter a Name</label>
					<input type="text" name="name" ng-model="user.name" required
						ng-focus='form.name.hasFocus=true', ng-blur='form.name.hasFocus=false'>
					<p ng-show="form.name.$error.required && (form.name.$touched || submitted)" 
						class="error-block">Name is Required!</p>
				</div>
			</form>
		</div>
	</div>

	<script src="bower_components/angular/angular.min.js"></script>
	<script src="app/app.js"></script>
</body>
</html>
```

###Compiled (Generated CSS)

```css
body {
    margin:0;
    padding:10px;
    background:#f9f9f9;
    font-family:helvetica,arial,sans-serif;
    box-sizing:border-box
}

.wrapper {
    width:100%
}

.form-wrapper {
    width:340px;
    margin:60px auto;
    padding:24px;
    background:#fff;
    box-shadow:2px 2px 2px rgba(0,0,0,0.1)
}

.form-group {
/*styles for the form-group itself*/
    position:relative;
    width:100%;
    margin-bottom:42px
}

.form-group input[type='text'] {
/*Default styles for field*/
    position:relative;
    width:100%;
    background:0 0;
    padding:26px 0 12px;
    font-size:15px;
    line-height:1.4;
    font-weight:500;
    border:1px solid rgba(0,0,0,0.08);
    border-width:0 0 1px;
    box-shadow:none;
    z-index:1
}

.form-group input[type='text']:focus {
    outline:0
}

.form-group label {
/*Default styles for label*/
    display:inline-block;
    position:absolute;
    margin-bottom:6px;
    top:0;
    font-size:12px;
    font-weight:300;
    color:rgba(0,0,0,0.4);
    padding:0;
    transition:all .3s ease
}

.form-group::after {
/*Default styles for 'after' pseudo selector for form-group*/
    content:"";
    position:absolute;
    width:0;
    height:2px;
    bottom:0;
    left:50%;
    z-index:1000;
    background:rgba(0,0,0,0.08);
    transition:all .3s ease
}

.form-group .error-block {
/*Default Styles for Help-block*/
    position:absolute;
    line-height:0;
    font-weight:300;
    font-size:12px;
    color:red;
    line-height:24px;
    transition:all .5s ease;
    margin:0
}

.form-group .error-block.ng-hide {
    display:block!important;
    visibility:hidden;
    opacity:0
}

.form-group.has-focus {
/*When field has focus*/
}

.form-group.has-focus label {
    color:#039BE5
}

.form-group.has-focus::after {
    left:0;
    background:#039BE5;
    width:100%
/*We are changing the width of the pseudo element from 0 to 100%. As there is already transition added to it, the change in width will 'animate'. As we have strategically positioned this pseudo element just below the input with a 2px height, it will 'appear' as bottom-border for the input (which it is actually not)*/
}

.form-group.has-error {
/*When field has error*/
}

.form-group.has-error label {
    color:red
}

.form-group.has-error::after {
    left:0;
    background:red;
    width:100%
}

.form-group.has-error .help-block:not(.ng-hide) {
    position:absolute;
    color:red;
    opacity:1;
    visibility:visible;
    top:100%
}

.form-group.has-success.has-focus {
/*When field has success*/
}

.form-group.has-success.has-focus label {
    color:#228b22
}

.form-group.has-success.has-focus.has-focus::after {
    left:0;
    background:#228b22;
    width:100%
}

.form-group.is-empty:not(.has-focus) label {
    font-weight:400;
    font-size:14px;
    top:32px
}

/*# sourceMappingURL=styles.css.map */
```

## Next Stpes

Following the steps above, you have successfully created a single input field which has floating-labels and basic error-handling. 

Now you can modify just the HTML markup to have more fields.

In this example, I have styled only the 'text' input field, with just minor additions/modification to the scss/css code you can style the other input fields as well.

In my [Example](http://gunua-works.com/air-pair/floating-labels/example/), I have just created another email field following same steps. Only change in the SCSS I had to do was to add `input[type='email']` along with the already defined `input[type='text']` selector.

## Source Code of the example

The source code of [this example](http://gunua-works.com/air-pair/floating-labels/example/) is available at this [Github repository](https://github.com/sambhav-gore/material-input-fields).
























