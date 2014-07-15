# CSS Tips & Tricks
We've compiled a list of the most commonly used CSS snippets that our engineers use at Mobify and we thought they'd be worth sharing. While we may not have come up with these ourselves, they're tried and tested in our mobile-focused labratories. Let the CSS's begin!

## Height 100% trick

### Scenario
You want to force an element (i.e. a wrapper or container div) to stretch to 100% height of the viewport.

### Problem
If you only set the desired element to height: 100%, you'll find that it won't be the height of the viewport at all.

### Solution
In order for height: 100% to work on containers, the html and body also need height:100% declared explicitly.

### Code
```
html, body {
    height: 100%;
}
```

## Center Vertically
### Scenario
You want to vertically center an element vertically.
### Problem
No real problem in this case. Maybe Browser compatibility for transform? 
### Solution
Use three properties on the element you want to vertically center. For more details, see this zerosixthree article.
### Code
```
.element {
    position: relative;
    top: 50%;
    transform: translateY(-50%);
}
```

## Clearfixes

### Scenario
A container's children are floated and are breaking outside the flow of the container. Using a clearfix on the container will help contain the children inside.

### Problem
There are many solutions to this. Which one do we use?

### Solution
By default, use overflow: hidden first.

### Code
```
.x-parent-container {
    overflow: hidden;
}
```

If, however, you require that something inside the container needs to be able to move outside (i.e. fixed or absolute positioning), then instead use Compass' pie-clearfix mixin.

```
.x-parent-container {
    @include pie-clearfix;
}
```

## Responsive Media Elements

### Scenario
When you want to embed responsive videos, iframes, images and more.
### Problem
You can't use just height, width and percentages alone to achieve responsive media elements.
### Solution
Use [Uncle Dave's Ol' Padded Box](http://daverupert.com/2012/04/uncle-daves-ol-padded-box/) trick, which uses background image, sets height to 0, and uses the magical padding-bottom that, when set with a percent value, achieves a responsive aspect ratio!

[http://daverupert.com/2012/04/uncle-daves-ol-padded-box/](http://daverupert.com/2012/04/uncle-daves-ol-padded-box/)

## Fluid perfect square
### Scenario
When you want fluid content containers that are responsive and also maintain their aspect ratio. 
### Problem
You can't use just height and width to achieve this effect. 
### Solution
Use this [padding-top trick](http://www.mademyday.de/css-height-equals-width-with-pure-css.html), similar to the padded box trick above which is fantastic for maintaining aspect ratio on containers too.
[http://www.mademyday.de/css-height-equals-width-with-pure-css.html](http://www.mademyday.de/css-height-equals-width-with-pure-css.html)

## Empty DOM Elements
### Scenario
There are empty elements on the page (i.e. <p> tags) that are pushing content down needlessly, and should really be hidden.
### Problem
You don't have control of the markup and there are no classes that you can target.
### Solution
You can use the :empty pseudo selector to hide empty elements in a page.
### Code
```
p:empty {
    display: none;
}
 
*:empty {
    display: none;
}
```

## Centering Modals
One generally reliable way to do these is to use table display.
Feel free to add others.

### Code
#### HTML
```
 <div class="modal">
  <div class="modal__backdrop">
    <div class="modal__dialog">
      <h1>Foo</h1>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Nulla sint incidunt ullam sapiente tenetur blanditiis sed magni voluptate nemo numquam explicabo optio totam voluptatem quod asperiores temporibus vero deserunt quam.</p>
    </div>
  </div>
</div>
```
#### CSS
```
* {
  box-sizing: border-box;
}
 
.modal {
  display: table;
  table-layout: fixed;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 999;
  width: 100%;
  height: 100%;
}
 
.modal__backdrop {
  display: table-cell;
  background-color: #333;
  background-color: hsla(0, 0%, 0%, 0.8);
  vertical-align: middle;
  text-align: center;
}
 
.modal__dialog {
  display: inline-block;
  width: 30em;
  max-width: 90%;
  background-color: #fff;
  padding: 1em;
}
```

[view a demo](http://codepen.io/ry5n/pen/lnjbh)

## Flexbox Browser Support
### Scenario
You want to use flexbox.
### Problem
You don't want to deal with all the vendor prefixes or legacy syntax.
### Solution
Use [this flexbox mixin](https://github.com/mastastealth/sass-flex-mixin).
Because the Compass' flexbox mixin is so bad, we've opted to use this mixin instead. Please use it if you're going to use flexbox in your projects.
[https://github.com/mastastealth/sass-flex-mixin](https://github.com/mastastealth/sass-flex-mixin)

## Lazy Loading Images In A Flexbox Layout
### Scenario
You are lazy loading images into a container that is a Flexbox.
### Problem
When images being lazy loaded in, the browser doesn't re-calculate Flexbox widths. 
### Solution
Unfortunately there is nothing we can do about it now  Let's keep an eye out for solutions.

## Text/Image Replacement
### Scenario
You want to accessibly replace text with an icon or image.
### Problem
The text must still be accessible to screen readers, etc. 
### Solution
Use text indent to hide the text.
### Code
```
.x-menu__icon{
    text-indent: 100%;
    &::after {
        text-indent: 0;
    }
}
```

## Replacing Break Tags
### Scenario
There are <br> tags that you want to remove or replace or collapse. They are also likely thrown into the middle text paragraphs, and if left will disturb the flow of content.
### Problem
The <br> tags can't be reliably targeted with JS. You want the content to flow naturally whether the breaks are there or not.
### Solution
The first is to simply replace the break tag with a single space.
### Code
```
br, br::after {
    content: ' ';
}
```

## Collapsing Break Tags
### Scenario
You want a cluster of <br> tags to behave as if there was only one.
For example: a cluster of <br> tags inbetween some paragraphs or headers, when there should really only be one.
### Problem
The <br> tags can't be reliably targeted with JS. You want the content to flow naturally whether the breaks are there or not.
### Solution
Replace the breaks with an empty block level pseudo-element with margins. That way, if there are multiple breaks, they all collapse into each other because they are empty and they are vertically spaced with margins (top and bottom margins can collapse into each other - think of headers and paragraphs).
### Code
```
br {
    content: '';
     
    display: block;
    margin-top: 1em;
}
```

## Webkit Overflow Scrolling
### Scenario
Trying to create momentum enabled, side-scrollable tabs inside an iframe.
### Problem
Overflow scrolling does not work when tabs are inside an iframe.
### Solution
When using the -webkit-overflow-scrolling property to create side-scrollable tabs that happens to appear in an iframe, you will find that the side-scrolling will cease to function.
### Code
```
// styles.css
.tabs-container {
    width: 100%;
    overflow: auto;
    -webkit-overflow-scrolling: touch;
    -webkit-backface-visibility: hidden;
}
```

Now with that in mind, we can use a small javascript snippet to disable and re-enable overflow, then disable and re-enable -webkit-backface-visibility

```
// scripts.js
$('.tabs-container').css('overflow', 'visible');
$('.tabs-container').attr('style', null);
$('.tabs-container').css('-webkit-backface-visibility', 'visible');
$('.tabs-container').attr('style', null);
  
// Note:
// I imagine there must be a more elegant solution to this script.
// If anyone thinks of something, please feel free to re-write this!
```

## Increasing Specificity
### Scenario
Need to increase specificity so our styles get applied
### Problem
`!important` should be used sparingly as a last resort, generally only to overwrite inline styles
### Solution
Use the `:nth-child` selector, with 1n for Android support
### Code
```
.class-name:nth-child(1n) {
  ...
}
```
