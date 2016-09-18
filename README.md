## Website Performance Optimization portfolio project

<<<<<<< HEAD
Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

### Getting started

####Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to the top-level of your project directory to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

####Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js. 

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>
=======
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
>>>>>>> gh-pages
