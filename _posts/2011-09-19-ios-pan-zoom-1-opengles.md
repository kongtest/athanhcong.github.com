---
layout: post
title: Pan/zoom in iOS (Part 1) - OpenGLES Setting
tags:
- English
- iPhone
- Software Development
- Technology
status: publish
type: post
published: true
meta:
  blogger_blog: athanhcong.blogspot.com
  blogger_author: Thanh Cong Vohttps://profiles.google.com/101084470848901147240noreply@blogger.com
  blogger_permalink: /2011/09/implement-panzoom-feature-with-multi.html
  _edit_last: '1'
  _wp_old_slug: pan-zoom-multi-touches-1
  al2fb_facebook_image_id: '511'
  _oembed_2f866d74f8913fabc98c2760bb438f24: <iframe width="584" height="329" src="http://www.youtube.com/embed/qSN5fPDinzs?fs=1&feature=oembed"
    frameborder="0" allowfullscreen></iframe>
  _oembed_39fb353524419cda860d04b745586e84: <iframe width="500" height="281" src="http://www.youtube.com/embed/qSN5fPDinzs?fs=1&feature=oembed"
    frameborder="0" allowfullscreen></iframe>
---
What is your first impression with the iPhone or any smart phone nowadays?
To me, that is how people touch their fingers to screen, use gestures to manipulate objects, like pan to move or pinch to zoom a map, a photo...

[caption id="attachment_511" align="alignnone" width="400" caption="Gestures like &quot;pan to move&quot; or &quot;pinch to zoom&quot; become industry standard. (Image source: top9tip.com)"]<a href="http://kong.vn/wp-content/uploads/2011/09/zoom-gesture.jpg"><img class="size-full wp-image-511 " title="zoom-gesture" src="http://kong.vn/wp-content/uploads/2011/09/zoom-gesture.jpg" alt="" width="400" height="266" /></a>[/caption]

This post will show you how to implement your own pan/zoom. Let's check how we can play with photos in Pic Collage and get some feeling what we are going to build.

http://www.youtube.com/watch?v=qSN5fPDinzs
<h1>1. Setting up OpenGLES</h1>
In this step, we want to load an image and put it in the iPhone screen. To do this, you know a litle about OpenGLES on iOS and how to load an image into that.
<pre>Texture------&gt; OpenGLES framebuffer ---&gt; view -&gt; iPhone screen</pre>
If you do not know anything about that, <a href="http://www.raywenderlich.com/5223/beginning-opengl-es-2-0-with-glkit-part-1" target="_blank">this tutorial</a> will be a good start.

So now, you know how to load an image into a texture and display it in 2D way.
You are getting this screenshot, after remove all of 3D and rotation stuff.
<a href="http://kong.vn/wp-content/uploads/2011/09/texture-opengl.png"><img class="alignnone size-full wp-image-512" title="texture-opengl" src="http://kong.vn/wp-content/uploads/2011/09/texture-opengl.png" alt="" width="232" height="349" /></a>

Let's go throught some other settings.
Change our viewport from Perspective to Orthographic. Learn how they are different <a href="http://iphonedevelopment.blogspot.com/2009/04/opengl-es-from-ground-up-part-3.html" target="_blank">from here</a>.
<pre lang="objc">    CGRect rect = self.view.frame;
    GLKMatrix4 projectionMatrix = GLKMatrix4MakeOrtho(-1.0,
           1.0,
           -1.0 / (rect.size.width / rect.size.height),
           1.0 / (rect.size.width / rect.size.height),
           0.01,
           10000.0);</pre>
<h1>2. How to make a simple pan/zoom/rotate?</h1>
We use several varitables to store the state of tranformation.
<pre lang="objc"> // Translate
 float _rotation;
 float _zooming;
 float _panX;
 float _panY;</pre>
Create a reset method and put it after setupGL in viewDidLoad method.
<pre lang="objc">- (void)resetTransform {
    _rotation = 0;
    _zooming = 1;
    _panX = 0;
    _panY = 0;
 }</pre>
Check GLKViewControllerDelegate's update method. You will see some interesting methods: GLKMatrix4MakeTranslation, GLKMatrix4Rotate. We will use these methods to make pan and rotate. Replace them with these:
<pre lang="objc"> GLKMatrix4 modelViewMatrix = GLKMatrix4MakeTranslation(_panX, _panY, -1); //for pan
 modelViewMatrix = GLKMatrix4Rotate(modelViewMatrix, _rotation, 0, 0, 1); //for rotate
 modelViewMatrix = GLKMatrix4Scale(modelViewMatrix, _zooming, _zooming, 1); // for zoom</pre>
And also create some new convenient methods:
<pre lang="objc">- (void)panWithVector:(CGPoint)vector {
    _panX += (float)vector.x;
    _panY += (float)vector.y;
}

static const CGFloat kZoomMinScale = 0.2;

- (void)zoomAtPoint:(CGPoint)center scale:(CGFloat)scale {
    _zooming *= scale;

    if (_zooming &lt; kZoomMinScale) {
        _zooming = kZoomMinScale;
    }
}

- (void)rotateWithAngle:(float)radian {
    _rotation += radian;

}</pre>
Now we can check how these method can make effect; put these methods, one at a time, after resetTransform, in viewDidLoad method,
<pre lang="objc"> [self panWithVector:CGPointMake(10, 10)];
 [self rotateWithAngle:GLKMathDegreesToRadians(45)];
 [self zoomAtPoint:CGPointMake(10, 10) scale:0.5];
 [self zoomAtPoint:CGPointMake(0, 0) scale:0.5];</pre>
<h1>4. How to zoom at any point?</h1>
You may notice that your image is always zoomed in center of image. Although, we want to zoom at a corner of an image, only center of image is shown, our corner goes out of screen. Looking back Pic Collage app or Google Maps all, you can zoom an image at any corner, any point in the middle of your finger, not just at the center.

Let's play with this a while to understand the problem. Put this method, in your GLKViewController
<pre lang="objc">- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event {
    //KONG: click any point in screen to double
    [self zoomAtPoint:[[touches anyObject] locationInView:self.view] scale:2];
}</pre>
This happens because our OpenGLES configuration always drawing texture at the center. After the image is zoomed, OpenGL keep drawing the image in centre. This make the corner goes out of screen.

How to keep that corner stay in screen when we want to zoom at it.
+ We first make a zoom, as usual, at center of screen.
+ Then, perform a pan to move the corner back to appropriate part of screen.

[caption id="attachment_518" align="alignnone" width="310" caption="We want to pan Ao back to A"]<img class="size-full wp-image-518" title="zoom-moving" src="http://kong.vn/wp-content/uploads/2011/09/zoom-moving.png" alt="" width="310" height="239" />[/caption]

This is a tough part: understand the hierachy of drawing a texture from OpenGL to UIKit view. Take a look at how we draw a texture again.
<pre>Texture------&gt; OpenGLES framebuffer ---&gt; UIView -&gt; iPhone screen</pre>
In the opposite way, when we touch a point in iPhone screen (1), the point is sent to UIView (2), then sent to OpenGL (3), then sent to texture drawing layer (4), here we make texture zoomed (5), and everything is sent back to screen again, through these layers.

I sketched some illustration:

<img class="alignnone  wp-image-513" title="OpenGL drawing" src="http://kong.vn/wp-content/uploads/2011/09/OpenGL-drawing.png" alt="" width="625" height="681" />

To know how to pan Ao back to A, we need to know where it is after zooming, to know that we will check how a point's coordination, for example A(0, 0) is transformed through these layers.

1. Let's assume we have an image, with a circle at the center. We can display this image on the screen.
2. Now we want to zoom at top-left corner of the screen (there has an X sign).
The point at top-left corner of the image may not be at top-left corner of image, but be near at the center; because the image may be zoomed and paned if these function is already active.

Here at UIView level, coordination origin is at top-left corner and our point have coordination is A(0,0).
3. At OpenGL layer, the coordiation origin is at bottom-left corner.
Our point have coordination A(0, 480)

4. At texture displaying layer, in this layer, a texture is drawed into OpenGLES context, and our texture is drawed at center.
At this layer, coordination origin is at center of screen.
Our point have coordination A(-160, 240)

5. We are zooming here, with scale 2x.
Our point A(-160, 240) is transformed to Ao(-320, 480)
<pre lang="objc">- (void)zoomAtPoint:(CGPoint)center scale:(CGFloat)scale {
    CGPoint A_w = center; // (2)
    CGPoint A_d = [self pointWithOutCameraEffect:A_w]; // (2)
    CGPoint A_gl = [self convertToGL:A_d]; // (3)
    CGPoint A_t = CGPointMake(A_gl.x - _screenSize.width/2, A_gl.y - _screenSize.height/2); // (4)

    CGPoint Ao_t = CGPointMake(A_t.x * scale, A_t.y * scale); // (5)
    CGPoint Ao_gl = CGPointMake(Ao_t.x + _screenSize.width/2, Ao_t.y + _screenSize.height/2); // revert of (4)
    CGPoint Ao_d = [self revertFromGL:Ao_gl];// revert of (3)
    CGPoint Ao_w = [self pointWithCameraEffect:Ao_d]; // revert of (2)

    //KONG: moving Ao back to A in UIView level
    [self panWithVector:CGPointMake((A_w.x - Ao_w.x), (A_w.y - Ao_w.y))];
    _zooming *= scale;
    if (_zooming &lt; kZoomMinScale) {
       _zooming = kZoomMinScale;
    }
 }</pre>
We perform panning back in UIView level, because the wrong display (Ao get shifted) happens in UIView.

The transforming method for step (2), (3) and (4) will be in the source code.

That's it! Congratz,

In next part, we will know how to <a title="Pan/zoom in iOS (Part 2) – Handle multitouch" href="http://kong.vn/2011/11/pan-zoom-multitouch-2/">handle multitouches and perform pan/zoom with fingers</a>.

You can get <a title="Pan/zoom with multitouch: TouchPhoto project" href="http://kong.vn/2012/02/pan-zoom-multitouch-project/">source code of completed project in this post</a>.
