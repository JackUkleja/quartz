---
title: Using d:DesignSource for design-time DataGrid grouping with sample data
date: 2010-06-18
layout: post
categories:
- wpf
tags: []
comments: true
aliases:
    - ../using-ddesignsource-for-design-time-datagrid-grouping-with-sample-data/
---


I recently had the need to use the row grouping feature of the WPF DataGrid. 
  
As you know, in order to design with Blend effectively, you really need to have data displayed at design-time. It’s fairly straight forward to use combinations of `d:DesignContext` and `d:DesignData` to achieve that goal. In the case of `DataGrid`, you can bind `ItemsSource` directly to a collection in your design-time model.
  
The question is: How do you use design-time data and grouping at the same time?
  
The reason the solution is not obvious is because the `DataGrid`’s grouping feature relies on having a `PropertyGroupDescription` configured for the `CollectionView` that the `DataGrid`'s `ItemSource` is bound to. If you bind `ItemsSource` directly to a collection, you do not have access to the `CollectionView` at design-time.
  
Just to confuse things, if you Google for [DataGrid grouping](http://www.google.com/search?q=datagrid+grouping) you will find a plethora of out-of-date articles suggesting you can configure the grouping by using the `DataGrid.GroupingDescriptions` in Xaml, which you can’t (it only worked on older/beta versions). On the other hand there are several articles which describe how to configure grouping programmatically. The problem with the programmatic approach is it does not run at design-time, and is in conflict with the design-time attributes. I want to use the well supported design-time sample data tool chain, and keep designers free from having to write code.
  
While Googling for a solution I came across the [new design-time attribute documentation](http://msdn.microsoft.com/en-us/library/ff602277(VS.95).aspx) (note: it applies equally well to WPF in spite of the Silverlight bias!), which included an attribute I had never seen before: `d:DesignSource`. According to MSDN, `d:DesignSource`:

> Specifies a design-time data source for a [CollectionViewSource](http://msdn.microsoft.com/en-us/library/system.windows.data.collectionviewsource(v=VS.95).aspx). This makes the designer aware of the shape of your types. This enables you to use the data binding builder to create bindings. 

and gives the following example usage:

    <CollectionViewSource x:Key="CustomerViewSource"
        d:DesignSource="{d:DesignInstance local:Customer, CreateList=True}" />

The description was intriguing but after looking at the example it seemed to be mistakenly underselling the capability. The sample code shows that it does more than make the "designer aware of the shape of your types" – it’s actually allowing sample data to be injected into a `CollectionViewSource` using the well know `d:DesignInstance` … and presumably `d:DesignData` too.

As it turns out, the only way to specify the `DataGrid` grouping in XAML is to use the `CollectionViewSource`. I figured by combining `CollectionViewSource` and `d:DesignSource` I could achieve my goals.

After some experimentation I got it working exactly as I hoped it would. Here is the code:

        <Grid>
    
            <Grid.Resources>
                <CollectionViewSource x:Key="groupedCollectionView"
                                      d:DesignSource="{d:DesignData Source=./SampleData/BookingRowCollectionSampleData.xaml }" 
                                      Source="{Binding RuntimeBookingRowCollection}">
                    <CollectionViewSource.GroupDescriptions>
                        <PropertyGroupDescription PropertyName="PropertyToGroupBy"/>
                    </CollectionViewSource.GroupDescriptions>
            </CollectionViewSource>
            </Grid.Resources>
    
            <DataGrid AutoGenerateColumns="True"
                      ItemsSource="{Binding Source={StaticResource groupedCollectionView}}" >
                <DataGrid.GroupStyle>
                    <x:Static Member="GroupStyle.Default"/>
                </DataGrid.GroupStyle>
            </DataGrid>
    
        </Grid>

There are a couple of things to point out:
  
* I used “Create sample data from class…” in Blend to create my `BookingRowCollectionSampleData.xaml`. The class which I generate from was defined below. I found I had to create this specialized collection in order to satisfy the `d:DesignSource`. Conversely my attempt at creating sample data based off my view model which in turn had a collection property was a dead end – `d:DesignSource` needs to be given a raw collection; there is no way to dereference the collection property within a view model. 
    
        public class BookingRowCollection : List<BookingRow>
        {
        }
  
* I wanted to make sure that I didn’t need multiple paths in my code behind to handle the differences between design-time and run-time, specifically with binding and MVVM. In this case the design-time “switch” is handled purely within the `CollectionViewSource`, where the Source is set differently depending on what mode we are in. Note that even though the `CollectionViewSource` is a resource, it will still correctly bind to our `RuntimeBookingRowCollection`. 

Declaring the `CollectionViewSource` proxy within your XAML will not suit all MVVM projects as sometimes you might want to define a `CollectionView` within your view model, but it worked in my case because the grouping was never going to change. For this project the grouping can be considered purely part of presentation so I am happy to declare it in XAML, and as a bonus it works beautifully with design-time sample data!

I think this solution will work for Silverlight too, but I have not tested it yet.