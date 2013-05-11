---
layout: post
title: 'Delaunay Triangulation applied in real-time on a WebCam stream'
categories:
- Web Development
tags:
- Video
- WebCam
- getUserMedia
- Canvas
type: post
published: false
---
I already detailed in a [previous post](/2012/09/frame-by-frame-video-effects-using-html5-and/) how typical image processing algorithms could be applied in real-time on a video stream using the HTML5 <code>canvas</code> to produce video effects.
 
In this article, I will explain how the same kind of effects can be applied on a WebCam stream thanks to the <code>getUserMedia</code> API.

<!--more-->

## Capturing WebCam frames in the Canvas

It is not possible (yet ?) to grab directly a WebCam frame to inject it in the HTML5 <code>canvas</code>: you have to go through an intermediary step, capturing first the stream in a <code>video</code> element, then transferring it frame by frame in the <code>canvas</code>.

As displayed below, we will use the [<code>getUserMedia</code>](http://dev.w3.org/2011/webrtc/editor/getusermedia.html) API to grab the video stream from a WebCam and inject it in a <code>video</code> element:

	navigator.getUserMedia({video: true}, 
						function(stream) {
							video.src = URL.createObjectURL(stream);
							// Process frames here
							...
                		}, 
                		function (error) {
                    		// Error handling
                		});

When the <code>getUserMedia</code> API is called, the user will be prompted to give access to its WebCam:

* acceptance will trigger the first function that creates an URL object from the stream and pass it to the video element,
* refusal will trigger the error function.

Once the stream has been successfully redirected to the <code>video</code> element, we will start capturing frames in the <code>canvas</code> at regular intervals timed by <code>requestAnimationFrame</code>. To make sure that there is actually something to be captured, we test against the <code>HAVE_ENOUGH_DATA</code> state for the <code>video</code> element before grabbing a frame. 

	function tick() {
		requestAnimationFrame(tick);
		
		if (video.readyState === video.HAVE_ENOUGH_DATA) {
			...
            ctx.drawImage(video, 0, 0, canvasWidth, canvasHeight);
            var imageData = ctx.getImageData(0, 0, cwidth, cheight);
			...
                    
Once a frame has been transferred to the <code>canvas</code>, it is extracted as a byte array to apply our image processing algorithms.
 
## Detecting the singularity points



## Applying Delaunay Triangulation


## Rendering back to Canvas

## Demo

Click on the image below to see how it works when all the pieces are put together:

<div class='picture'>
<a href='http://kaizouman.github.io/js-delaunay-effect/'>
<img src='/demos/delaunay-effect/delaunay-effect.jpg'>
</a>
</div>



 