# 3812-ar

Look up the work of Stu Eve, if you want to see really thoughtful explorations of AR for archaeology. His [early video about 'augmenting a Roman fort'](https://vimeo.com/30861262) ([blog post](https://www.dead-mens-eyes.org/augmenting-a-roman-fort/)) still blows my mind:

https://vimeo.com/30861262

## What are we building?

Print out, or open in another browser window, 000_diary1.png and 001_diary2.png.

On your phone, open https://shawngraham.github.io/3812-ar.

When it asks for permission to use your camera, say 'yes'. Then when the scanner animation starts, point your camera at either of the two images. You'll see two 'ghosts' that are now haunting these diary pages. Cool, eh?

The code for the webapp we'll build below is adapted from [here](https://hiukim.github.io/mind-ar-js-doc/examples/multi-targets).

## Make target images aka tracking images

The three .png images in this repo can work as tracking images. Right-click and save as image if you want to use them. Then, when the webapp is complete, print them out, load up your app on your phone and test! Alternatively, if you don't have a printer, you can open them in a browser, and then test your webapp by loading it on your phone & pointing at the screen. But let's not get ahead of ourselves. To make your own tracking images:

1 - decide what your target images are going to be (ie the things that you are going to augment)

2 - rename them in numerical order, 000_target.png, 001_target.png etc

3 - upload all at once at [https://hiukim.github.io/mind-ar-js-doc/tools/compile](https://hiukim.github.io/mind-ar-js-doc/tools/compile)

4 - examine the results - look for the red circles to indicate that the tool has found many points of interest. more is always better. If it looks pretty sparse, you might have to modify your images (pick new ones, increase contrasts on the existing ones) and try again

5 - double check that your images are in the correct order 

6 - download the targets.mind file


## Get the Basic Repo
7 - go to this repo and hit the 'fork' button to _clone_ my repository to yours.

8 - replace my trackers with your own: upload the targets.mind file so that it overwrites mine. You should have something like this:

```
|
|-gravestone 2
|-index.html
|-readme.md
|-targets.mind
```

## Add your image targets

9 - we're going to edit the index.html file so that we swap out the existing image targets for the ones you just created. But first I want you to notice a few things.

In the <head> part of the file - the code between <head> and </head> tags, there is a series of <script> tags that are telling the browser to go to these locations, and get the javascript code that knows how to do AR. DO NOT MESS WITH ANYTHING HERE. For reference, they look like this:

```

  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.1.5/dist/mindar-image.prod.js"></script>
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@v6.1.1/dist/aframe-extras.min.js"></script>
    <script src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.1.5/dist/mindar-image-aframe.prod.js"></script>
    <script src="video-handler.js"></script>
  </head>

```

The augmented reality consists of us telling the browser where to find the tracking images (the image targets) and what kind of content to associate with them.

10 - Look for this line within the <body> section of the index.html:

```
<a-scene mindar-image="imageTargetSrc: https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.1.5/examples/image-tracking/assets/band-example/band.mind;" color-space="sRGB" renderer="colorManagement: true, physicallyCorrectLights" vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
```

The first bit `<a-scene` tells the browser, 'here are the parameters for doing AR'. The next element, `mindar-image` tells the browser where to find the tracking images. Right now, in this example, it is set to find a file called `band.mind` at a different website. But since we just uploaded our file to the same location as this index.html, we can strip out that https://cdn etc to just be our target.mind file. Do that now. **nb** keep the final ;", and do not alter any of the other elements in this line. It'll look like this:

```
<a-scene mindar-image="imageTargetSrc: targets.mind;" color-space="sRGB" renderer="colorManagement: true, physicallyCorrectLights" vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
```

11 - Now commit your changes. **If** you want your app to track more than one image at a time (ie, display two or more augmentations at once, if two or more trackers are visible), modify with maxTrack: `imageTargetSrc: targets.mind; maxTrack: 2"`

## make your web app live

12 - Go to the repo settings, and under the Pages option, make your repo live on the web.

## test it

13 - It'll take a couple of minutes, but once the repo is published on github.io, load the page on your phone or tablet. If everything is good, you'll be asked to allow the website to access your camera. Say yes. If you can see through the camera, and you get a scanning animation, look at your tracking image so that the image is in the middle of the frame. Your augmentation should appear! 

But if you get a blue screen with three loading dots that doesn't disappear after a few seconds or so, and your camera view doesn't appear, there's an error in your code you need to fix. Quotation marks and semi-colons etc are where typos often can creep in; check your code.

## How does the code attach the AR asset to the tracking image?

If you look at the code for the basic experience, you'll see that inside the <a-scene> </ascene> tags we've also defined some assets within the `<a-assets>` tag, `<a-asset-item>` and `<a-video>`. For each one, we have assigned an id (so we can reference it later). The first is graveModel, and it references a .gltf file of mine that I downloaded from Sketchfab.com. I downloaded the zip file, and placed the whole unzipped folder inside this repo. The `src="` bit points to the .gltf file inside that folder - but for the .gltf file to work, you need everything else that's in that folder too.

The next `<a-asset-item>` points to a location outide this repo with another gltf file. The final `<a-video>` points to a video that is IN this repo; you can't hotlink to youtube. 

```
      <a-assets>
        <a-asset-item id="graveModel" src="gravestone2/scene.gltf"></a-asset-item>
        <a-asset-item id="raccoonModel" src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.1.5/examples/image-tracking/assets/band-example/raccoon/scene.gltf"></a-asset-item>
        <video id="testMovie" autoplay="true" preload="auto" src="me.mp4" loop="true" crossOrigin="anonymous" webkit-playsinline playsinline</video>
      </a-assets>

```

Having defined our assets, we now tell the webapp which tracking image to go to. Remember that the computer starts counting at 0 rather than 1, which is why I reminded you to name your files `000_image.png,` `001_image.png` etc. 

```
      <a-entity mindar-image-target="targetIndex: 0">
        <a-gltf-model rotation="0 0 0 " position="0 -0.25 0" scale="0.05 0.05 0.05" src="#graveModel" animation-mixer>
      </a-entity>
      <a-entity mindar-image-target="targetIndex: 1">
        <a-gltf-model rotation="0 90 0 " position="0 -0.25 0" scale="0.05 0.05 0.05" src="#bearModel" animation-mixer>
      </a-entity>
        <a-entity mindar-image-target="targetIndex: 2" material="shader: flat; src: #testMovie" geometry="primitive: plane; width: 4; height: 2;" position="0 0 -20" rotation="0 35 0" video-handler>
      </a-entity>  
  ```

Here, we use `<a-entity mindar-image-target="targetIndex: 0">`  to say, hey, here's how we want you to display the graveModel on the screen or hey, we want you to attach the video to the third target (targetIndex: 2) and lay it out like this.

**The third image is not tied to any asset in index.html** So if you point the camera at the third image, nothing will happen. For a challenge, see if you can figure out how to get a third _thing_ of some sort loaded up. [Check out this documentation](https://hiukim.github.io/mind-ar-js-doc/). Do you see how you could add other assets to your webapp, and how to tie them to the different target images? Look up the documents for mindar or aframe if you want to see how else you can add things.

Or go to [sketchfab.com](https://sketchfab.com) and see if there are any models that you are permitted to download; get the .gltf zip, unzip, and put the folder in your repo. Then do you see how you'd modify the code to define the asset and attach it as an entity to the app?

## Location Based AR
        
See this [from my digital archaeology course](https://digiarch.netlify.app/week/11/augmented-reality/) to get a handle on making your AR appear based on location, using the same framework we've used above.

It is also possible to write geotriggers in Twine such that a passage appears only if you're standing in the right spot. See [The Twine Cookbook](https://twinery.org/cookbook/geolocation/sugarcube/sugarcube_geolocation.html). Download the example; import it into Twine. See also [this older tutorial](https://github.com/shawngraham/ar-archaeology/blob/master/workshop%20materials/Hacking%20Twine%20to%20make%20a%20location-based%20game.md) about how to set your different passages by geolocation. If you go to [my demo scratchpad repo](https://github.com/shawngraham/demo/blob/master/twine-ar/cu-arg.html) you will see the code for 'cu-arg.html'. Click on 'raw', and copy all of that code to a text editor like sublime text or notepad++. Then, save as 'twine-ar.html' or similar. 

Try it out - go to the quad, and then [load it up in your browser](https://shawngraham.github.io/demo/twine-ar/cu-arg) (eg, on your phone!) https://shawngraham.github.io/demo/twine-ar/cu-arg.

pen up the twine editor, and import that html. You can now modify the locations and passages as per my older tutorial. The key thing is the init passage that sets up the coordinates for your passages, and the calibrate scan passage that looks to see if anything matches, and displays the relevant text. (I should mention that I tested this on firefox on android and it works.)
