---
title: 'Expression Blend 3 Help: The missing DelegateCommand class'
date: 2009-11-01
layout: post
categories:
- Uncategorized
tags: []
comments: true
---


Some tutorials from the Expression Blend 3 help docs (e.g. **“Try it: Display data from a sample SQL database”,** which is based on these [two](http://msdn.microsoft.com/en-us/library/cc294789.aspx) [articles](http://blogs.msdn.com/expression/articles/528008.aspx)**)** require a class that is present in the ColorSwatch WPF sample. The problem is that someone appears to have forgotten to bundle the ColorSwatch sample with the Blend 3!
  
To save you the hassle of trying to track down the DelegateCommand class here it is:
  
    using System; 
    using System.Collections.Generic; 
    using System.Text; 
    using System.Windows.Input; 
     
    namespace ColorSwatch 
    { 
        public sealed class DelegateCommand : ICommand 
        { 
            public delegate void SimpleEventHandler(); 
     
            private SimpleEventHandler handler; 
     
            private bool isEnabled = true; 
     
            public DelegateCommand(SimpleEventHandler handler) 
            { 
                this.handler = handler; 
            } 
     
            #region ICommand implementation 
     
            void ICommand.Execute(object arg) 
            { 
                this.handler(); 
            } 
     
            bool ICommand.CanExecute(object arg) 
            { 
                return this.IsEnabled; 
            } 
     
            public event EventHandler CanExecuteChanged; 
     
            #endregion 
     
            public bool IsEnabled 
            { 
                get { return this.isEnabled; } 
                set 
                { 
                    this.isEnabled = value; 
                    this.OnCanExecuteChanged(); 
                } 
            } 
     
            private void OnCanExecuteChanged() 
            { 
                if (this.CanExecuteChanged != null) 
                { 
                    this.CanExecuteChanged(this, EventArgs.Empty); 
                } 
            } 
        } 
    } 