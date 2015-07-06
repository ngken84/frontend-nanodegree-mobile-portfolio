## Website Performance Optimization portfolio project

####Part 1: Optimize PageSpeed Insights score for index.html

I ran the page through page speed insights and got a 28/100 on Mobile and a 30/100 on Desktop. Not a great start but that's what I'm here for. Let's start with the easy stuff:

#####1. Eliminate render-blocking JavaScript and CSS in above-the-fold content.

Pretty straight forward. Add a media query to the print css and make the load blocking resources asynchronous. In line analytics script was pulled out to a separate JS file so it can be made async. 

#####2. Optimize Images

Optimized images using : https://compressor.io/compress
Also created separate icon image for pizzaria image because fullsize pizza image was 2MB.

#####3. Minimize CSS, JS and HTML

Minimize CSS: http://cssminifier.com/
Minimize JS: http://jscompress.com/
Minimize HTML: http://www.willpeavy.com/minifier/

#####4. Steps that will be taken server sides.

Leverage browser caching: Configure the js and css files with "cache-control" and "ETag" so that files wou be cached and not retrieved upon subsequent loads. 

####Part 2: Optimize Frames per Second in pizza.html

###Measure

Measuring the page on a Nexus 7 under two conditions show significant areas where the page's performance drops below 60 FPS. 
1. When the page is scrolling and the background pizzas are moving back and forth.
2. When the pizza size is toggled.

###Optimize

####1. Optimizing scrolling
There are two places that need to be fixed during the optimization. First, ensure that the images are on marked for their own layer. To do this, we identify the class that these background cookies are a part of and add some css to them to ensure they are on their own layer.

```bash
  .mover {
    will-chnage : transform;
    transform : translateZ(0);
  }
```

Next we have to deal with the layout thrashing in the update code. The function that updates the location of the cookies is the updatePositions function

In the for loop, the following code forces layout to be run repeatedly:
```bash
  var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
```
To reduce the amount of layout thrashing, this code is pulled out of the loop and only performed once. 

