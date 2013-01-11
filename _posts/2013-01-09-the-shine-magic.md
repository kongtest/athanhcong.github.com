---
layout: post
title: The Misfit Shine's Magic - Put device on iPhone screen to transfer data.
tag: technology, iphone,

---

If you haven't know The Shine yet, watch this video:
<iframe width="640" height="360" src="http://www.youtube.com/embed/1RL8PjiOoGI" frameborder="0" allowfullscreen></iframe>


Pretty cool, right? It's small and elegant, it's useful and look firmly with metal. However the most attracted point is how it send data to the phone. As in the video: "All you need to do is place the Shine on your smartphone, the Shine synchonizes with your phone… No wire, cable, or pair. Simple!"

Wow, it likes magic, or to me, such an enlightenment moment!

How? How can the Shine do that? 

I started by listing some of the options:

- Sound, maybe some kind of ultrasound? But sound can be effect by the environment. In fact some sport devices use sound, like Polar RS200, but it has a not nice noise.

- Bluetooth or Wifi? No, because normally you need to pair Bluetooth device with iPhone. Actually with Bluetooth 2.1+, you can skip the pairing process, by using "[Just Work](http://stackoverflow.com/a/6023413/192800)" specification. But I check the demo, there is no Bluetooth icon on the iPhone's status bar, also noticed that the metal body may make this imposible.

- NFC, not on iPhone yet.

After searching around I found a *very possible answer*: Transfer data through **Capacitive** touch screen.

Capacitive touch screen uses the fact that in our body have a small electric current. When we touch, the screen "recognize this electricity". The good thing is that we can fake a finger touch. Using a device to fake the touch, we can send a series of signals as we want, to the iPhone app.

We also need a protocol (aka rules) for transfering data. The simplest way is using binary, a series of 0 and 1: 010000111

The iPhone application will receive these signals, decode it to meaningful data, and finally visually display your result: _"You did 12 points today, you are super man!"_ :D

If you even **want to build a new Shine, read this [paper](http://www.winlab.rutgers.edu/~gruteser/papers/tammob12.pdf)**. I haven't finished it, but it's worth to cite it here. 

Now, I'm thinking about a magic ring, which I can point to the screen to unlock my laptop or iPhone. No password, nor PIN code. Simple! Haha :)


To sum up, knowing this doesn't lose my interest in the device. Anyway, revert something is much easier than think it out and make it attractive. 

I still really love how simple and elegant the device is. As a Vietnamese, I love it even more! Look forward to see it success.