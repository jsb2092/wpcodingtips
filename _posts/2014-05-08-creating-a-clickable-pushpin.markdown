---
layout: post
title:  "Creating a pushpin on a map"
date:   2014-05-08 17:41:20
tags: maps c# phone "8.1"
category: maps
platform: phone
---

With the new Windows Phone RT API, there are new ways to create maps, add items to the maps, and generally just deal with maps.  One of the best things, in my opinion, is the fact that they got rid of the ugly pushpin that WP7 and WP8 had.  With this tutorial, I'm going to show you how to make a custom image pushpin that is clickable.

I definted 3 private variables.  These are used to reference some items outside the local block, for moving the pin:
{% highlight c# %}
private Windows.UI.Xaml.Shapes.Rectangle fence;
private BasicGeoposition fencePos;
private MapControl myMap;
{% endhighlight %}

my loaded function then looks like this:

{% highlight c# %}
private void MyMap_OnLoaded(object sender, RoutedEventArgs e)
{
    myMap = (MapControl) sender;
    myMap.Center = new Geopoint(new BasicGeoposition()
    {
        Altitude = 643, 
        Latitude = 43.089863,
        Longitude = -77.669609
    });
    myMap.ZoomLevel = 14;
    fencePos = new BasicGeoposition()
    {
        Latitude = 43.089863, 
        Longitude = -77.669609
    };
    var point = new Geopoint(fencePos);

    fence = new Windows.UI.Xaml.Shapes.Rectangle
    {
        Width = 30, 
        Height = 30
    };

    var img = new BitmapImage(new Uri("ms-appx:///Assets/redpin.png"));
    fence.Fill = new ImageBrush()
    {
        ImageSource = img
    };

    MapControl.SetLocation(fence, point);
    MapControl.SetNormalizedAnchorPoint(fence, new Point(1.0, 0.5));

    myMap.Children.Add(fence);

    fence.Tapped += (o, args) =>
    {
        var myDialog = new MessageDialog( "You clicked on something");
        myDialog.ShowAsync();
    };
}

{% endhighlight %}
