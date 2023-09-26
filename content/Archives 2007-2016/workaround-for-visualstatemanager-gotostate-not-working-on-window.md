---
title: Workaround for VisualStateManager.GoToState not working on Window
date: 2009-10-14
layout: post
categories:
- Troubleshooting
tags:
- wpf
comments: true
aliases:
    - ../workaround-for-visualstatemanager-gotostate-not-working-on-window
---

I am not sure if this is a bug or what, but I’ve had an annoying problem today whereby it seems you are unable to switch states procedurally using code like this:

    VisualStateManager.GoToState(this, "MyState", true);
when your control (the first parameter) is not a UserControl. In my case that control was the top level Window of my WPF application.

The strange thing is the state switching works fine when I use the **GoToStateAction**.

After pouring over the code in Reflector it strikes me that something I don’t yet understand is happening with CustomVisualStateManager which Blend actually sets to be ExtendedVisualStateManager.

It seems ExtendedVisualStateManager has a method called GoToElementState which allows you to pass in a FrameworkElement, whereas GotToState only allows a Control.

By trial and error I found that the following code works:

    ExtendedVisualStateManager.GoToElementState(this.LayoutRoot as FrameworkElement, "MyState", true);
where Layroot root is a Grid and is the first child of my Window, and is where the actual VisualStateGroups are defined.

If anyone has an idea what is going on here, and why the GoToState doesn’t work as expected for a Window, please let me know.

<span style="color: #0000ff;"><strong>Update: <a href="http://connect.microsoft.com/Expression/feedback/ViewFeedback.aspx?FeedbackID=509740#details">According to a comment from Steve at Microsoft</a>, this issue will be fixed when the Visual State Manager gets integrated into .NET Base Class Libraries (BCL) e.g. when WPF 4.0 is released.</strong></span>