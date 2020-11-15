# Experimental Tutorial

Animation, while not required for a website, is something that can help make a site have that extra *flair* that draws a user in and keeps them scrolling. 
You can spend hours upon hours creating a beautiful layout with stunning images and designing a sharp typography design, all in the pursuit of making a website look great. Animations are what help it *feel* great.

Here, I am going to show you how to make two different elements for a website with compelling (and cool) animations - a parallax image header and a timeline that reveals its contents after the user scrolls onto them. 
This tutorial does assume basic understanding of HTML/CSS and Javascript, so assuming you at least know your way around the basics, let's get started.  

<div align="center">
  <img src="https://cdn.acodez.in/wp-content/uploads/2018/01/Website-Animation-When-and-How-to-Use-It.png" />  
</div>
<div align="center"><sub><sup>https://acodez.in/animation-website-design/</sub></sup></div>
<br />
<br />
  
## Element 1 - Parallax Header Image

A parallax image is one that gives a sense of perspective as you scroll by making different layers of the image scroll at faster or slower speeds.

The first and most important piece of this is having your artwork separated into different layers, which each layer being a different distance away from the "camera" position. Of course, this is easiest if the image was something like a digital painting with everything already nicely separated. In my case, however, the image I wanted to use was just one from the internet.

>Separating an image into different layers is most commonly done with a program like Photoshop, but that's an entire process on its own that goes outside the scope of this tutorial. So, if that's something you're curious about, [this video](https://www.youtube.com/watch?v=H7g_-ix9J5I&ab_channel=eHow) is a helpful starting point! *(It's a bit older and uses an outdated version of Photoshop, but the process is still the same as today).*

For this tutorial, I've provided an image that has already been separated into its own layers. (My layer cutting wasn't perfect, but that's because this focuses more on the code and less on the imagery). 

*A key thing to remember is to make sure your layers are PNGs with transparent backgrounds, not white. If they aren't transparent, the effect won't work!*

<br />

### Initial Setup

The HTML section of this is actually pretty simple. We're going to create a parent container and give it an ID of 'main-banner.' Inside that parent container we're going to make one div for each layer in the image and give it the class 'layer.' We're also going to give each div a data-type attribute with the value "parallax."

```
<div id="main-banner">
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
        <div class="layer" data-type="parallax"></div>
</div>
```
<br />

Now it's time to add some basic styling. We'll start with the parent container.

```
#main-banner {
    height: 1080px;
    overflow: hidden;
    position: relative;
}
```
<br />

We'll also add styling for the 'layer' class. Two key things here are that they need to be the same height as #main-banner and have a position: fixed. 

```
.layer {
    background-position: top center;
    background-repeat: no-repeat;
    background-size: contain;
    height: 1080px;
    position: fixed;
    width: 100%;
    z-index: -1;
}
```
<br />

Now that that's done, it's time to add the actual imagery. We need to create a class for each individual layer and place the URL of that image inside the "background-image" property.

```
.layer-01 {
   background-image: url(‘input_link_to_image_here');
}
.layer-02 {
   background-image: url(‘input_link_to_image_here');
}
// etc.
```
<br />

After that, return to the HTML and update each layer div with the proper class. The first layer will be the background and the rest will stack on top of each other accordingly.

```
<div id="main-banner">
        <div class="layer-1 layer" data-type="parallax"></div>
        <div class="layer-2 layer" data-type="parallax"></div>
        <div class="layer-3 layer" data-type="parallax"></div>
        
        etc.
</div>
```
<br />

### Animating with Javascript

The first step to making this into an animation begins with Javascript. All of the Javascript for this element will go in one singular function. 
Let's start by adding the ability to check if the user is scrolling.

```
(function() {
  window.addEventListener(‘scroll’, event);

}).call(this);
```
Here, the EventTarget.addEventListener() method registers the specified listener on the event target it’s called on. This target could be any element in the site, the entire site itself, or any other object that supports events.
<br />

Now we need to store the number of pixels that the site has been scrolled into the *topDistance* variable. We can use *pageYOffset* for this. Your code should now look something like this:

```
(function() {
  window.addEventListener(‘scroll’, function(event) {
    var topDistance;
    return topDistance = this.pageYOffset;
  });

}).call(this);
```
<br />

After that, we can take each layer and store it into a variable called *'layers.'* We can do this with *querySelectorAll* and the data-attribute that we put inside our HTML earlier.

```
(function() {
  window.addEventListener(‘scroll’, function(event) {
    var layers, topDistance;
    topDistance = this.pageYOffset;
    return layers = document.querySelectorAll("[data-type='parallax']");
  });

}).call(this);
```
<br />

Now that that's done, we'll need to go back to our HTML and make one more change. Each layer needs to have a new value called *"data-depth"* specified. This is what controls how much or how little each layer moves as the user scrolls. 

Before actually going back to the HTML, however, we'll begin by making something to loop through all the layers and apply the necessary *transform* to each, according to where the user has scrolled to.
We'll create a *for* loop and start it by creating a variable where we'll store our layers. Then, we'll have it take the aforementioned *"data-depth"* value that's going to go in the HTML.

```
  var depth, layer, _i, _len;

  for (_i = 0, _len = layers.length; _i < _len; _i++) {
    layer = layers[_i];
    depth = layer.getAttribute(‘data - depth’);
  }
  ```
  <br />
  
 After that, we'll have the browser calculate the movement of the layers by multiplying the distance from the top of the page by our specified *"data-depth."* A layer with a value of 1.0 will scroll with the page like any normal static image, basically without any parallax-ing. All values less than 100 (or 1.0) will have a parallax effect that increases as the value decreases.

```
movement = -(topDistance * depth);
```
<br />

The final step is to update the final value of movement to each layer's CSS paramater *"transform translate3d,"* giving it its own 'location,' in a sort of 3d sense.

```
 translate3d = 'translate3d(0, ' + movement + 'px, 0)';
            layer.style['-webkit-transform'] = translate3d;
            layer.style['-moz-transform'] = translate3d;
            layer.style['-ms-transform'] = translate3d;
            layer.style['-o-transform'] = translate3d;
            layer.style.transform = translate3d;
```
<br />

All in all, your JS code should look about like this:

```
(function() {
    window.addEventListener('scroll', function(event) {
        var depth, i, layer, layers, len, movement, topDistance, translate3d;
        topDistance = this.pageYOffset;
        layers = document.querySelectorAll("[data-type='parallax']");
        for (i = 0, len = layers.length; i < len; i++) {
            layer = layers[i];
            depth = layer.getAttribute('data-depth');
            movement = -(topDistance * depth);
            translate3d = 'translate3d(0, ' + movement + 'px, 0)';
            layer.style['-webkit-transform'] = translate3d;
            layer.style['-moz-transform'] = translate3d;
            layer.style['-ms-transform'] = translate3d;
            layer.style['-o-transform'] = translate3d;
            layer.style.transform = translate3d;
        }
    });
}).call(this);
```
