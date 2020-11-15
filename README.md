# frostpunk-tutorial

Animations, while not required in a website, are just some of things that can help make a website have that extra flair that draws a user in and keeps them scrolling. 
You can spend hours upon hours creating a beautiful layout with stunning images and designing a sharp typography design, all in the pursuit of making a website look great. Animations are what help it feel great.

Here, I am going to show you how to make two different elements for a website with compelling (and cool) animations. This tutorial does assume basic understanding of HTML/CSS and Javascript, so assuming you at least know your way around the basics, let's get started.



Element 1 - Parallax Header Image

A parallax image is one that gives a sense of perspective as you scroll by making different layers of the image scroll at faster or slower speeds. 
The first and most important piece of this is having your artwork separated into different layers, which each layer being a different distance away from the "camera" position. Of course, this is easiest if the image was something like a digital painting with everything already nicely separated. In my case, however, the image I wanted to use was just one from the internet. 
Using Photoshop (or something similar) to separate an image into separate layers goes beyond the scope of this tutorial, so if that's something you're curious about, (https://www.youtube.com/watch?v=H7g_-ix9J5I&ab_channel=eHow) this video is a helpful starting point (it's a bit older and uses an outdated version of Photoshop, but the process is still the same as today). For this tutorial, I've provided the layers I used in the project in a folder in this repo (my layer cutting wasn't perfect, but again, that's because I was focusing more on the code and less on the image). A key thing to remember is to make sure your layers are PNGs with transparent backgrounds, not white. If they aren't transparent, the effect won't work!

HTML

The HTML section of this is actually pretty simple. We're going to create a parent container and give it an ID of 'main-banner.' Inside that parent container we're going to make one div for each layer in the image and give it the class 'layer.' We're also going to give each div a data-type attribute with the value "parallax."

<div id="main-banner">
        <div class="layer-1 layer" data-depth="0.05" data-type="parallax"></div>
        <div class="layer-11 layer" data-depth="0.20" data-type="parallax"></div>
        <div class="layer-2 layer" data-depth="0.1" data-type="parallax"></div>
        <div class="layer-3 layer" data-depth="0.15" data-type="parallax"></div>
        <div class="layer-4 layer" data-depth="0.25" data-type="parallax"></div>
        <div class="layer-5 layer" data-depth="0.35" data-type="parallax"></div>
        <div class="layer-6 layer" data-depth="0.60" data-type="parallax"></div>
        <div class="layer-7 layer" data-depth="0.70" data-type="parallax"></div>
        <div class="layer-8 layer" data-depth="0.80" data-type="parallax"></div>
        <div class="layer-9 layer" data-depth="0.90" data-type="parallax"></div>
        <div class="layer-10 layer" data-depth="1.00" data-type="parallax"></div>
</div>

Now it's time to add some basic styling. We'll start with the parent container.

```
#main-banner {
    height: 1080px;
    overflow: hidden;
    position: relative;
}
```

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

After that, return to the HTML and update each layer div with the proper class. The first layer will be the background and the rest will stack on top of each other accordingly.
