---
title: An Auto-Centering Canvas for WPF
date: 2010-02-26
layout: post
categories:
- wpf
tags: []
comments: true
---


If you try using WPF Canvas to plot objects in a 2d “scene”, you will hit a small problem. The Left and Top dependency properties dictate where the top left corner of your element is positioned. Not so good if you want to plot your elements at or over the coordinate position.
  
Here is a simple way to extend the Canvas such that Top and Left position your elements around their center.
  
        public class CenteringCanvas : Canvas
        {
            protected override Size ArrangeOverride(Size arrangeSize)
            {
                foreach (UIElement element in base.InternalChildren)
                {
                    if (element == null)
                    {
                        continue;
                    }
                    double x = 0.0;
                    double y = 0.0;
                    double left = GetLeft(element);
                    if (!double.IsNaN(left))
                    {
                        x = left - element.DesiredSize.Width/2;
                    }
                    
                    double top = GetTop(element);
                    if (!double.IsNaN(top))
                    {
                        y = top - element.DesiredSize.Height/2;
                    }
                    
                    element.Arrange(new Rect(new Point(x, y), element.DesiredSize));
                }
                return arrangeSize;
            }
        }
Of course don't expect this code to work if you try setting the Right or Bottom properties.

