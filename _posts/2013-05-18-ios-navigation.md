---
layout: post
title: Choosing Navigation UI for your app. Tab Bar vs Sidebar?
tag: startup, ui, ux, ios, navigation

---
_To use Tab Bar or to use Sidebar, that is the question?_

Tab Bar navigation is a standard navigation in iOS, which can be found in Twitter app or Instagram app. While Sidebar navigation is a newer trend, you can find in popular app like Facebook or Path.

![](/images/2013/instagram-tabbar.png)
![](/images/2013/facebook-sidebar.png)

It's interesting that 4 apps above (Twitter, Instagram, Facebook, Path)are all social apps, but they have different ways to present available functions in the app.  How did each app choose their UI? If you are building your new iOS app, which kind of navigation will you use?

Let's consider some of the differences between these 2 UI patterns:

# Space taken: Tab Bar vs Sidebar: 0 - 1
It's obvious that sidebar hides the navigation options to the left side, so it save lots of space for main content. On other hand, Tab Bar always takes some space at the bottom of you screen.

Doing Sidebar way, you want to focus users into 1 main view, 1 main action, which usually is scrolling through the feed. Everything else is optional.

# Visibility: Tab Bar vs Sidebar: 1 - 0
As a result of taking space, Tab Bar presents more functional options users can have. What is benefits? It hugely increases the chance that these funtions can be discovered and used daily.

If your app have more than 1 functions that can be used daily, Tabbar is a good choice.

From here, I start to consider that __Tabbar is more suitable for mobile-first app, while Sidebar is more suitable for consuming app__ (app which is a consuming version of another web version). I will talk more about this later...

![](/images/2013/path-sidebar.png)
![](/images/2013/twitter-tabbar.png)

# Accessibility (Physically and Usability): Tab Bar vs Sidebar: 1 - 0
Is it easy for users to select the menu in Tab Bar and Sidebar?

Due to the way we hold our phone, Tab Bar, which is placed at the bottom of the screen, makes it easier to touch. All Tabs are visible, so it's even faster. 

With Sidebar, it's much harder for your finger to reach the Sidebar button at top-left corner. There usually have many options to choose after that, check Facebook app. It requires lots of thinking for 1 selection.


# Getting back: Tab Bar vs Sidebar: 1 - 0
Is it easy for users to get back to default view, after using the app for a while?

With Sidebar, it's easy to lose the navigation context: where are you?
Open Facebook app, open Sidebar, choose Messages then choose any conversation, now you will feel you are in a very strange app.

Then, continue with Facebook app, choose the [i] button in top-right corner, choose the person profile. You are navigating deeply into a stack of views, how do you get back to the Sidebar menu? For most of the people I know, they will click [Back] button for 4 times, to reveal the menu. Lost!

![](/images/2013/facebook-stack-of-view.png)

To fix this, Sidebar give you ability to swipe to left to access Sidebar, no matter where you are. With this gesture, you don't need to reach your finger to top-left button any more, perform a swipe is much easier. However normal users don't know about this swipe-to-reveal gesture.

With Tab Bar, most of the time, your Tabs will be there for you to select. To navigate back in a stack of view, just touch the same tab twice. This is easier to discover than Sidebar's swipe-to-reveal.

![](/images/2013/facebook-swipe-to-reveal.png)
![](/images/2013/instagram-tab-go-back.png)

# Action button: Tabbar vs Sidebar: 1 - 0
For some app, creating new content is sometime more important as browsing content.

In Instagram app, people open the app when they want to share a photo. At some moment, users want to access camera as quickly as posible. Instagram solve that problem with a big button in their Tab Bar. 

Path app, add their action button at the bottom-right corner. It's a good solution, but it will not give me a firmly feeling like Instagram button.


# Summary
In mobile app, where user experience is very important, it takes you time to decide which UI pattern is suitable for your app, which makes your app natural to use, easy to remember, convienient to access?

Howerver if you need a quick reference, I would say that Tab Bar is more suitable for mobile-first (social) app. Where creating content in app is more important than browsing content and you need to show users that you have more than 1 useful features.

Sidebar is suitable for consuming app, where content can be created outside the app.