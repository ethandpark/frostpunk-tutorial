# frostpunk-tutorial

Animation, while not required for a website, is something that can help make a site have that extra *flair* that draws a user in and keeps them scrolling. 
You can spend hours upon hours creating a beautiful layout with stunning images and designing a sharp typography design, all in the pursuit of making a website look great. Animations are what help it *feel* great.

Here, I am going to show you how to make two different elements for a website with compelling (and cool) animations - a parallax image header and a timeline that reveals its contents after the user scrolls onto them. 
This tutorial does assume basic understanding of HTML/CSS and Javascript, so assuming you at least know your way around the basics, let's get started.  
  
  
  
## Element 1 - Parallax Header Image

A parallax image is one that gives a sense of perspective as you scroll by making different layers of the image scroll at faster or slower speeds.

The first and most important piece of this is having your artwork separated into different layers, which each layer being a different distance away from the "camera" position. Of course, this is easiest if the image was something like a digital painting with everything already nicely separated. In my case, however, the image I wanted to use was just one from the internet.

>Separating an image into different layers is most commonly done with a program like Photoshop, but that's an entire process on its own that goes outside the scope of this tutorial. So, if that's something you're curious about, [this video](https://www.youtube.com/watch?v=H7g_-ix9J5I&ab_channel=eHow) is a helpful starting point! *(It's a bit older and uses an outdated version of Photoshop, but the process is still the same as today).* 

For this tutorial, I've provided an image that has already been separated into its own layers. (My layer cutting wasn't perfect, but that's because this focuses more on the code and less on the imagery). 

**A key thing to remember is to make sure your layers are PNGs with transparent backgrounds, not white. If they aren't transparent, the effect won't work!

HTML

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

```
<div id="main-banner">
        <div class="layer-1 layer" data-type="parallax"></div>
        <div class="layer-2 layer" data-type="parallax"></div>
        <div class="layer-3 layer" data-type="parallax"></div>
        
        etc.
</div>
```
