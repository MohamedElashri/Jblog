---
published: true
layout: post
title: "Convert ROOT to CSV"
description: "How to convert ROOT files to CSV files"
comments: true
keywords: "ROOT, data, LHC, CERN, csv"
---

-----------------------


Recently I'm working with a lot of `.root` files which is the standard format used in high energy physics for data storage and analysis. It is still hard to use specialized machine learning tools to read and analyze this data. So I searched for a solution and came into this amazing package [rootpy](https://www.rootpy.org/). 

The rootpy package is a Python interface to the ROOT data analysis tool. It provides a convenient way to work with ROOT files and trees in Python.

To convert a ROOT file to a CSV file using rootpy, you can use the `root2array()` function. This function takes the path to the ROOT file and the name of the tree as arguments, and returns the data from the tree as a NumPy array. You can then use the NumPy `savetxt()` function to save the array to a CSV file.

Here is an example of how to convert a ROOT file to a CSV file using rootpy:

``` python
import rootpy
import numpy as np

# Load the data from the ROOT file into a NumPy array

data = rootpy.root2array("path/to/file.root", "tree_name")

# Save the array to a CSV file

np.savetxt("output.csv", data, delimiter=",")
```

In this example, the `root2array()` function loads the data from the specified tree in the ROOT file and returns it as a NumPy array. The savetxt() function then saves the array to a CSV file with the specified delimiter (in this case, a comma).

Note that the `root2array()` function only converts the data from the tree to a NumPy array. If you want to include the branch and leaf names in the CSV file, you will need to use a different approach.


Also we can use the following code: 

``` python
import rootpy
from rootpy import root_open
f=root_open('File.root')
t=f.Get('FlatTree')
t.csv()
```
This code uses the rootpy package to open a ROOT file named File.root and extract the data from a tree named FlatTree. The `root_open()` function is used to open the ROOT file, and the `Get()` method is used to access the tree.

Once the tree is accessed, the `csv()` method is called to export the data from the tree to a CSV file. This method writes the data to a CSV file with the same name as the tree and saves it in the same directory as the ROOT file.

For example, if the ROOT file is located at `/path/to/File.root` and the tree is named FlatTree, the CSV file will be saved at `/path/to/FlatTree.csv`.

The `csv()` method has several optional arguments that can be used to customize the output. For example, you can specify the delimiter to use in the CSV file, the precision of the floating point numbers, and whether to include the branch and leaf names in the output.