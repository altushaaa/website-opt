## Website Performance Optimization portfolio project

The challenge was to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

### Getting started

###Optimization
####Part 1: Optimize PageSpeed Insights score for index.html

1. Hosted the project website with GitHub Pages on gh-pages branch (very useful tutorial on https://pages.github.com/)
2. First ran the URL through PageSpeed Insights! Got score of 27/100 for Mobile :(
3. Applied the optimizations suggested by PageSpeed Insights:
* Removed render-blocking css and JavaScript in three ways

Used media query for print.css:
```html
<link href="css/print.css" rel="stylesheet" media="print">
```

Minified style.css with cssminifier.com and in-lined in html:
```html
<style>html{font-size:100%;overflow-y:scroll;-webkit-tap-highlight-color:rgba(0,0,0,0);-ms-text-size-adjust:100%;-webkit-text-size-adjust:none}body{margin:0;font-size:14px;line-height:1.61;font-weight:400}body,button,input,select,textarea{font-family:'Open Sans',sans-serif;color:#333}a{color:#12C}a:visited{color:#61C}a:focus{outline:thin dotted}a:hover,a:active{color:#c00;outline:0}b,strong{font-weight:700}pre,code{font-family:monospace,monospace;font-size:1em}ul,ol{margin:1em 0;padding:0 0 0 20px}img{b...(line truncated)...
```

Moved GoogleFonts css to the bottom of the document

* Applied `async` property to non render-critical JavaScript:
```html
<script src="http://www.google-analytics.com/analytics.js" async></script>
```

* Optimized image sizes - downloaded images compressed by the PageSpeed Insights

* Additionally, minified js/perfmatters.js file.

Profiled, optimized, measured every step of the way... and then lathered, rinsed, and repeated. Got both Mobile and Desktop PageSpeed score of over 90! Great stuff!

####Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, I modified views/js/main.js until your frames per second rate is 60 fps or higher for both scrolling and re-sizing.

1. Replaced ` document.querySelectorAll` and ` document.querySelector` (expensive) selectors with `getElementsByClassName` and `getElementById` through the whole file

2. Moved (as much as found it possible) all calculations and `var` definitions outside of the FOR-loops as they do not need to be defined in every iteration of the loops (Source: [This discussion forum](https://discussions.udacity.com/t/having-trouble-getting-60fps/188782/3)) in the following functions:
* `changePizzaSizes`
* `updatePositions`

3. Following the logic in point 2 above, moved `var` defintions outside of functions where these variables are defined only once per load and not needed to be re-defined every time the function runs:
* `var pizzaSize = document.getElementById("pizzaSize");` was moved outside of `changePizzaSizes` function;
* `items` was moved outside of `updatePositions` and calculated only once when the sliding pizzas are created upon load:
```javascript
var item=[];
// Generates the sliding pizzas when the page loads.
document.addEventListener('DOMContentLoaded', function() {
  ...
  //Sliding pizzas are only generated once when the page loads - it makes sense to also generate the array once, not everytime the updatePositions function fires
  //Source: https://discussions.udacity.com/t/having-trouble-getting-60fps/188782/3
  items = document.getElementsByClassName('mover');
  updatePositions();
});
```

4. Reduced the number of sliding pizzas from 200 to tailor to the user's screen height:
```javascript
//at each moment of time only a set number of scrolling pizzas show up on the screen simultaneously - so we can limit their number
var numPizzas = (screen.height/100) * cols;
console.log(numPizzas);
for (var i = 0; i < numPizzas; i++) {
```

5.  Replaced `style.left` with `style.transform` property to reduce the need for re-triggering the Layout
```javascript
for (var i = 0; i < movingPizzas; i++) {
  items[i].style.transform = 'translateX('+ (100 * phase[i%5]) + 'px)';
}
```
