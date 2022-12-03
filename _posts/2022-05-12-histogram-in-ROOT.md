---
published: true
layout: post
title: "Histograms in ROOT "
description: "How to plot/draw histograms in ROOT CERN"
comments: true
keywords: "ROOT, Gauss, LHC, CERN, Histogam, Plot, Draw"
---

-----------------------


ROOT is a popular data analysis tool used in the field of high energy physics. It is a powerful tool that allows users to perform a variety of tasks, including generating histograms. In this article, we will show you how to generate histograms in ROOT.

To create a histogram in ROOT, you first need to have a data set that you want to plot. This data can be stored in a ROOT file or it can be generated directly in your ROOT session. Once you have your data, you can use the TH1 class to create a histogram. This class has several constructors that allow you to specify the number of bins, the range of the data, and other options.

For example, to create a histogram with 10 bins ranging from 0 to 100, you can use the following code:

``` c++
TH1F h("h", "My Histogram", 10, 0, 100);
```

The `TH1F` class is used for histograms with floating point data, while the `TH1D` class is used for histograms with double precision data. You can also use the `TH1I` class for histograms with integer data.

Once you have created your histogram, you can fill it with data using the `Fill()` method. This method takes the value of the data point as an argument and increments the bin corresponding to that value. For example, to fill the histogram with random values between 0 and 100, you can use the following code:

``` c++
for (int i = 0; i < 1000; i++) {
  h.Fill(gRandom->Uniform(0, 100));
}
```

The gRandom object is a global random number generator provided by ROOT. The `Uniform()` method generates a random number between the two specified values (in this case, 0 and 100).
Once you have filled your histogram with data, you can draw it on a canvas using the `Draw()` method. This method takes the name of the histogram and the drawing options as arguments. For example, to draw the histogram with a bar chart style and a blue color, you can use the following code:

``` c++
h.Draw("BAR", "color=blue");
```

The `BAR` option specifies that the histogram should be drawn as a bar chart, while the `color=red` option specifies that the bars should be red. There are many other drawing options available, such as `LINE`, `SCAT`, and `LEGO`, which can be used to customize the appearance of the histogram. 

To draw a histogram as a line chart, you can use the LINE option:

``` c++
h.Draw("LINE");
```

In addition to changing the drawing style, you can also customize the colors of the histogram. The `Draw()` method also takes a string argument that specifies the color. For example, to draw a histogram in red, you can use the following code:

h.Draw("BAR", "color=red");



You can also add labels and titles to the histogram to make it more informative. To add a title to the histogram, you can use the `SetTitle()` method. This method takes the title as an argument. For example, to add the title "My Histogram" to the histogram, you can use the following code:

``` c++
h.SetTitle("My Histogram");
```

To add labels to the x-axis and y-axis, you can use the `GetXaxis()` and `GetYaxis()` methods, respectively. These methods return an TAxis object that can be used to customize the labels. For example, to add the labels "X-axis" and "Y-axis" to the x-axis and y-axis, respectively, you can use the following code:

``` c++
h.GetXaxis()->SetTitle("X-axis");
```

In addition to these options, the TH1 class provides many other methods and options that can be used to customize histograms. For more information, you can refer to the [ROOT documentation](https://root.cern.ch/doc/master/).