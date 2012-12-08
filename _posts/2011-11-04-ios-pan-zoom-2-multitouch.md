---
layout: post
title: Pan/zoom in iOS (Part 2) – Handle multitouch
tags:
- English
- iPhone
- Mathematics
- Software Development
- Technology
- User eXperience
status: publish
type: post
published: true
meta:
---
In <a title="Pan/zoom in iOS (Part 1) – Setting OpenGLES" href="http://kong.vn/2011/09/pan-zoom-multitouch-1/" target="_blank">previous post of this series</a>, we know how to pan, rotate and zoom at any point with OpenGL. This post will help you glue all of these internal functions with natural multitouch gestures.

Firstly, we need to know <strong>how are touches passed into code?</strong>
By these delegate:
    
    - (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event;
    - (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event;
    - (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;
    - (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event;</pre>
  
If you are not familiar with these methods, play with it a little bit. Put above method into your GKLViewController, then add below lines into touchBegan method:

    NSLog(@"touchesBegan: %@", NSStringFromCGPoint([[touches anyObject]locationInView:self.view]));
    NSLog(@"touches: %d", [touches count]);
    NSLog(@"event: %d", [[event allTouches] count]);

##Pan to move?
Add these lines into touchMove:

    NSArray *allTouches = [[event allTouches] allObjects];
    UITouch *touch = [allTouches objectAtIndex:0];
    CGPoint pointA_ = [touch locationInView:self.view];
    CGPoint pointA = [touch previousLocationInView:self.view];
    [self panWithVector:CGPointMake(pointA_.x - pointA.x, pointA_.y - pointA.y)];

When a finger is moving in screen, it forms a vector whose start point and end point is location of the finger before and after move. What we did in this code is getting location of that touch after moving (line 3) and before moving (line 4). Then we pan with vector formed from these 2 points. Let's build and see.

##Pinch to zoom?
"Pinch to zoom" can be performed by 2 fingers. When talking about a zoom, we need to talk about its scale and center point. Scale ratio is easy to figure out, it is the ratio of how far your 2 fingers are after and before moving.
Let's say before moving, your 2 fingers are at A and B, after moving, your 2 finger are at A_ and B_. The ratio will be A_B_/AB.

__How about center point?__ It appears to be at the center of your 2 fingers, but actually it does not, most of the time.

To find it, firstly let's perform some UI tests on Google Maps app to see how really "pinch to zoom" gesture do?

1. Put 2 fingers on screen, pinch to zoom. Zoom is performed with scale: how far your finger is expanded.

<img class="alignnone size-full wp-image-531" title="multitouches-test1" src="http://kong.vn/wp-content/uploads/2011/11/multitouches-test1.jpg" alt="" width="320" height="210" />

The image is enlarged with a scale A'B'/AB  
2. Hold a finger on screen, move another, the map's zoomed, 2 marks is still under your finger.

<a href="http://kong.vn/wp-content/uploads/2011/11/multitouches-test3.jpg"><img class="alignnone size-full wp-image-533" title="multitouches-test3" src="http://kong.vn/wp-content/uploads/2011/11/multitouches-test3.jpg" alt="" width="320" height="144" /></a>

3. In Maps, if you put your first finger on location A, another finger on location B, after preforming pan/zoom, both locations are still right under your finger, no matter how many time you zoom out, then in.
  
<img class="alignnone size-full wp-image-532" title="multitouches-test2" src="http://kong.vn/wp-content/uploads/2011/11/multitouches-test2.jpg" alt="" width="320" height="115" />

2 point O and X are still under 2 fingers after zooming

4. If 2 fingers touch the screen at different timestamp, you still can zoom.

5. Pan and zoom can be performed at the same time
6. 
  <a href="http://kong.vn/wp-content/uploads/2011/11/multitouches-test4.jpg"><img class="alignnone size-full wp-image-534" title="multitouches-test4" src="http://kong.vn/wp-content/uploads/2011/11/multitouches-test4.jpg" alt="" width="320" height="195" /></a>

6. Twist 2 finger around to form a circle, the map is not panned or zoomed
  
<a href="http://kong.vn/wp-content/uploads/2011/11/multitouches-test5.jpg"><img class="alignnone size-full wp-image-535" title="multitouches-test5" src="http://kong.vn/wp-content/uploads/2011/11/multitouches-test5.jpg" alt="" width="320" height="128" /></a>

2 point O and X are still in the same place

<strong>Conclusion</strong>: a natural "pinch to zoom" gesture will keep 2 marks stay under your fingers in most cases.

Now, pan/zoom gestures seem clearer, but how to put these UI tests into code. We gonna translate previous conclusion into Mathematics language:
<strong>Equivalent Mathematical problem</strong> for pan/zoom feature.
<blockquote>On a plan, given 2 pair of points A, B and A', B'. How to use <a href="http://www.mathsisfun.com/geometry/transformations.html" target="_blank">transformations (translation, resizing, rotation)</a> to move A to A' and B to B'?</blockquote>
Below are my step by step solution:

<img class="alignnone size-full wp-image-548" title="multitouches-transforms2" src="http://kong.vn/wp-content/uploads/2011/11/multitouches-transforms2.jpg" alt="" width="472" height="335" />

- First. move A to A' by using a Translation with vector AA', B is also moved to B1
- Second, move B1 to B2 by using a resize with origin in A', scale A'B2/A'B1 = A'B'/AB
- Finally, use a Rotation with origin A', angle (A'B2, A'B') to make vector A'B2 to same direction with A'B', B2 is moved to B'

Next step, we go to implement all of these.
<pre lang="objc">- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event {
    //KONG: move from point A to A_
    NSArray *allTouches = [[event allTouches] allObjects];

    if ([allTouches count] == 1) {
        UITouch *touch = [allTouches objectAtIndex:0];
        CGPoint pointA_ = [touch locationInView:self.view];
        CGPoint pointA = [touch previousLocationInView:self.view];
        [self panWithVector:CGPointMake(pointA_.x - pointA.x, pointA_.y - pointA.y)];
    } else if ([allTouches count] == 2) {

        UITouch *touchA = [allTouches objectAtIndex:0];
        UITouch *touchB = [allTouches objectAtIndex:1];

        CGPoint pointA_ = [touchA locationInView:self.view];
        CGPoint pointA = [touchA previousLocationInView:self.view];

        CGPoint pointB_ = [touchB locationInView:self.view];
        CGPoint pointB = [touchB previousLocationInView:self.view];

        //- First. move A to A’ by using a Translation with vector AA’, B is also moved to B1
        CGPoint vectorAA_ = [self vectorFrom:pointA to:pointA_];
        [self panWithVector:vectorAA_];

        // Calculate B1
        CGPoint pointB1 = [self translatePoint:pointB withVector:vectorAA_];

        //- Second, move B1 to B2 by using a resize with origin in A’, scale A’B'/A’B1
        CGFloat scale = [self distanceFrom:pointA_ to:pointB_] / [self distanceFrom:pointA_ to:pointB1];
        [self zoomAtPoint:pointA_ scale:scale];

        //- Finally, use a Rotation with origin A’, angle (AB, A’B') to make vector A’B2 to same direction with A’B', B2 is moved to B’
        CGPoint vectorAB = [self vectorFrom:pointA to:pointB];
        CGPoint vectorA_B_ = [self vectorFrom:pointA_ to:pointB_];
        [self rotateWithAngle:[self angleFrom:vectorAB to:vectorA_B_]];
    }
}</pre>
I think the implementation is clear enough now.

You can get <a title="Pan/zoom with multitouch: TouchPhoto project" href="http://kong.vn/2012/02/pan-zoom-multitouch-project/">source code of completed project in this post</a>.
