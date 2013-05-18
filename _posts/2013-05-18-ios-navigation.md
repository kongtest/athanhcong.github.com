---
layout: post
title: Choosing Navigation UI for your app: Tab Bar vs Sidebar?
tag: startup, ui, ux, ios, navigation

---
To use Tab Bar or to use Sidebar, that is the question?

Tab Bar navigation is a standard navigation in iOS, which can be found in Twitter app or Instagram app. While Sidebar navigation is a newer trend, you can find in popular app like Facebook or Path.

![](/images/2013/instagram-tabbar.png)
![](/images/2013/twitter-tabbar.png)
![](/images/2013/facebook-sidebar.png)
![](/images/2013/path-sidebar.png)

It's interesting that 4 apps above are all social apps, but they have different ways to present available functions in the app  How did each apps choose their UI? If you are building your new iOS app, which kind of navigation will you use?

These are some ideas I learnt when making choice for my Challenge App.

# Space taken: Tab Bar vs Sidebar: 0 - 1
We start with an obvious element: Sidebar hides the navigation options to the left side, so it save lots of space for main content. On other hand, Tab Bar always takes some space at the bottom of you screen.

Doing Sidebar way, you want to focus users into 1 main view, 1 main action, which usually is scrolling through the feed. Everything else is optional.

# Visibility: Tab Bar vs Sidebar: 1 - 0
As a result of taking space, Tab Bar presents more functional options users can have. What is benefits? It hugely increases the chance that these funtions can be discovered and used daily.

If your app have more than 1 functions that can be used daily, Tabbar is a good choice.

From here, I start to consider that Tabbar is more suitable for mobile-first app, while Sidebar is more suitable for consuming app (app which is a consuming version of another web version). I will talk more about this later...

# Accessibility (Physically and Usability): Tab Bar vs Sidebar: 1 - 0
Is it easy for users to select the menu in Tab Bar and Sidebar?

Due to the way we hold our phone, Tab Bar which is placed at the bottom of the screen, make it easier to touch. All Tabs are visible, so it's even faster. 

With Sidebar, it's much harder for your finger to reach the Sidebar button at top-left corner. There usually have many options to choose after that, check Facebook or Path app. It requires lots of thinking for 1 selection.

# Getting back: Tab Bar vs Sidebar: 1 - 0
Is it easy for users to get back to default view, after using the app for a while?

With Sidebar, it's easy to lose the navigation context, where are you?
Open Facebook app, open Sidebar, choose Messages and choose any conversation, now you will feel you are in a very strange app.

Now, continue with Facebook app, choose the [i] button in top-right corner, choose the person profile. You are navigation deeply into a stack of views, how do you get back to the navigation menu? For most of the people I know, they will click [Back] button for a 4 times, to reveal the menu. Lost!

![](/images/2013/facebook-stack-of-view.png)

To fix this, Sidebar give you ability to swipe to left to access Sidebar, regardless where you are (with some twist in implementation). With this gesture, you don't need to reach your finger to top-left button any more, you only need to swipe.

With Tab Bar, most of the time, your Tabs will be there for you to select. To navigate back in a stack of view, just touch the same tab twice. This is easier to discover than Sidebar's swipe-to-reveal.

![](/images/2013/facebook-swipe-to-reveal.png)
![](/images/2013/instagram-tab-go-back.png)

# Action button: Tabbar vs Sidebar: 1 - 0
For some app, creating new content is also as important as consuming content.

With Instagram app, 1 main reason that people open the app is when they want to share a photo. At some moment, users want to access camera as quickly as posible. Instagram solve that problem with a big button in their Tab Bar. 

Path app, add their action button at the bottom-right corner. It's a good solution, but it will not give me a firmly feeling like Instagram button.


# Summary
In mobile app, where user experience is very important, it takes you time to decide which UI pattern is suitable for you. Ask yourself which one is natural to use, easy to remember, convienient to access?

If you need a quick reference, I would say that Tab Bar is more suitable for mobile-first (social) app. Where creating content in app is more important than browsing content and you need to show users that you have more than 1 useful features.

Sidebar is suitable for consuming app, where content can be create outside the app.