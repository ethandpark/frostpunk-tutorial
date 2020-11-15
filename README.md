# Experimental Tutorial

Animation is something that can help make a site have some extra *flair* that draws a user in and keeps them scrolling. Static websites are great, don't misunderstand. You can spend hours upon hours creating a beautiful layout with stunning images and a sharp typography design, all in the pursuit of making a website look great. Animations, however, are what help it *feel* great.

Here, I am going to show you how to make two different elements for a website with compelling (and cool) animations - a parallax image header and a timeline that fades-in its contents after the user scrolls onto them. 
This tutorial does assume basic understanding of HTML/CSS and Javascript. So, assuming you at least know your way around the basics, let's get started.  

<div align="center">
  <img src="https://cdn.acodez.in/wp-content/uploads/2018/01/Website-Animation-When-and-How-to-Use-It.png" />  
</div>
<div align="center"><sub><sup>https://acodez.in/animation-website-design/</sub></sup></div>
<br />
<br />
  
## Element 1 - Parallax Header Image

A parallax image is one that gives a sense of perspective by making different layers of the image scroll at faster or slower speeds.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/d/d7/Parallax_scroll.gif" />  
</div>
<div align="center"><sub><sup>https://commons.wikimedia.org/wiki/File:Parallax_scroll.gif/</sub></sup></div>

The first and most crucial part of this is having your artwork separated into *separate transparent* layers; each layer will be a different distance away from the "camera." Of course, this is easiest if the picture you're using has already been separated into its own layers, like a digitial painting. In my case, however, the one I wanted to use was already flattened into a single image.

>###### Separating an image into different layers is most commonly done with a program like Photoshop, but that's an entire process on its own that goes outside the scope of this tutorial. If that's something you're curious about, [this video](https://www.youtube.com/watch?v=H7g_-ix9J5I&ab_channel=eHow) is a helpful starting point! It's a bit older and uses an outdated version of Photoshop, but the process is still the same with today's versions.

For this tutorial, I've provided a sample image with the layers already separated. (My layer cutting wasn't perfect, but that's because I'm focusing more on the code and less on the imagery).  
Please note that the imagery in this tutorial does **not** belong to me in any way and total ownership of it belongs to [11Bit Studios](https://www.11bitstudios.com/about-us/). 
   
Alright, with that out of the way, let's start coding.

  
  
<br />  

### Initial Setup

The HTML section of this is actually pretty simple. We're going to create a parent container and give it an ID of 'main-banner.' Inside that parent container we're going to make one div for each layer in the image and give it the class "layer." We're also going to give each div a data-type attribute with the value "parallax."

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

Now it's time to add some basic styling. We'll start with the parent container. The *height* should be whatever your image is.

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
.layer-1 {
   background-image: url(‘input_link_to_image_here');
}
.layer-2 {
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
>###### Here, *EventTarget.addEventListener()* registers the specified listener on the event target it’s called on. This target could be any element in the site, the entire site itself, or any other object that supports events.

<br />

Now we need to store the number of pixels that the site has been scrolled into the *topDistance* variable. We can use *pageYOffset* for this.
```
  window.addEventListener(‘scroll’, function(event) {
    var topDistance;
    return topDistance = this.pageYOffset;
  });
```
<br />

After that, we can take each layer and store it into a variable called *'layers.'* We can do this with *querySelectorAll* and the data-attribute that we put inside our HTML earlier. So far, our function should look something like this:

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

Now that that's done, we'll need to go back to our HTML and make one more change. Each layer needs to specify a new value called *"data-depth."* This is what controls how much or how little each layer moves as the user scrolls. Don't worry about the actual value for now though, we'll come back to it in a second.

```
<div id="main-banner">
        <div class="layer-1 layer" data-depth="0.10" data-type="parallax"></div>
        <div class="layer-2 layer" data-depth="0.20" data-type="parallax"></div>
        <div class="layer-3 layer" data-depth="0.30" data-type="parallax"></div>
        
        etc.
</div>
```
<br />

Let's go back to our JS now to get that new value set up. We'll begin by making something to loop through all the layers and apply the necessary *transform* to each, according to where the user has scrolled to.

Let's create a *for* loop and start it by creating a variable where we'll store our layers. Then, we'll have it take the aforementioned *"data-depth"* value.

```
  var depth, layer, _i, _len;

  for (_i = 0, _len = layers.length; _i < _len; _i++) {
    layer = layers[_i];
    depth = layer.getAttribute(‘data - depth’);
  }
  ```
  <br />
  
After that, we'll have the browser calculate the movement of the layers by multiplying the distance from the top of the page by our specified *"data-depth."* A layer with a value of 1.0 will scroll with the page like any normal static image, basically without any parallaxing. All values less than 1.0 will have a parallax effect that *increases* as the value *decreases.*

```
movement = -(topDistance * depth);
```
<br />

The final step is to update the final movement value to each layer's CSS paramater *"transform translate3d,"* giving it its own 'location,' in a sort of 3d sense.

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
<br />

Now we can go back to the HTML and mess around with the *"data-depth"* values until we're happy with the results.
Note that when you're working with an image that you had to manually separate layers for in an external photo-editor like Photoshop, this can get a little finicky. You may have to do a little bit of extra work in your software and add some content-aware "filler" to make sure that when the page scrolls, the image doesn't leave empty space when it is transformed. 

You can see this in the images I've supplied for this tutorial - many of them (especially the more background ones) have clearly had a lot of [clone-stamping](https://helpx.adobe.com/photoshop/how-to/clone-stamp-remove-object.html) done to them, though it's only obvious when looking at the layer by itself. If that clone-stamping had not been done, those parts of the image would have been totally transparent and the parallax illusion would be broken, since the background color of the site would have shown through those spots.

If that doesn't make much sense, you can see what I mean by using these images and setting each data-depth at intervals of one tenth (so .10, .20, .30, etc.). It gets funky, and not in a good way.

<br />

On that note, for these images, here's what my HTML ended up looking like:

```
<div id="main-banner">
   <div class="layer-1 layer" data-depth="0.05" data-type="parallax"></div>
   <div class="layer-2 layer" data-depth="0.1" data-type="parallax"></div>
   <div class="layer-3 layer" data-depth="0.15" data-type="parallax"></div>
   <div class="layer-4 layer" data-depth="0.25" data-type="parallax"></div>
   <div class="layer-5 layer" data-depth="0.35" data-type="parallax"></div>
   <div class="layer-6 layer" data-depth="0.60" data-type="parallax"></div>
   <div class="layer-7 layer" data-depth="0.70" data-type="parallax"></div>
   <div class="layer-8 layer" data-depth="0.80" data-type="parallax"></div>
   <div class="layer-9 layer" data-depth="0.90" data-type="parallax"></div>
</div>
```

Again, if you have direct access to the artwork you're using and can manipulate the layers directly, then you're already streets ahead. It is possible to take any random image from Google and make it work, but as you can see, it's a much more involved process.
