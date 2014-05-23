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
    // I get a handle to the map - this was created using a hub app
    // which, for some reason does not give you a handle to the 
    // variable, so we do everything in onLoaded
    myMap = (MapControl) sender;

    // pick an arbitrary point to display the map, so we can see the pin
    fencePos = new BasicGeoposition()
    {
        Latitude = 43.089863, 
        Longitude = -77.669609
    };
    myMap.Center = new Geopoint(fencePos);
    myMap.ZoomLevel = 14;
    
    // create a point for the marker
    var point = new Geopoint(fencePos);

    // we create a rectangle here, because shapes have a 
    // Tapped event, and we can set the fill to an image
    // 30x30 is big enough to hold our square image
    fence = new Windows.UI.Xaml.Shapes.Rectangle
    {
        Width = 30, 
        Height = 30
    };

    // load the image and assign it to the fill of the shape
    var img = new BitmapImage(new Uri("ms-appx:///Assets/redpin.png"));
    fence.Fill = new ImageBrush()
    {
        ImageSource = img
    };

    MapControl.SetLocation(fence, point);
    MapControl.SetNormalizedAnchorPoint(fence, new Point(1.0, 0.5));

    // add the shape to the map as a child element
    myMap.Children.Add(fence);

    // create an anonymouse method to show a dialog when you tap it
    fence.Tapped += (o, args) =>
    {
        var myDialog = new MessageDialog( "You clicked on something");
        myDialog.ShowAsync();
    };
}

{% endhighlight %}
