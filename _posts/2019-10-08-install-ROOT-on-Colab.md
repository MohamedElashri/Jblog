---
published: true
layout: post
title: "ROOT on Colab"
description: "How to install and use ROOT on Google Colab"
comments: true
keywords: "ROOT, Colab, Jupytrer, LHC, CERN, Notebook"
---

-----------------------


Several months ago, I needed to use ROOT on Google Colab. I found a few suggesetions on the web, but none of them worked for me. So, I decided explore and try to make it work myself tutorial. In this article, I will show you how to install ROOT on Google Colab and how to use it in colab Jupyter notebook session.

First I did the build from source part that is completely described in the [ROOT installation guide](https://root.cern/install/build_from_source/). and uploaded the ROOT folder to a github repository. Then I created a new colab notebook and added the following code to the first cell:

```bash
!wget https://github.com/MohamedElashri/HEP-ML/releases/download/ROOT/ROOT.tar.zip
!unzip /content/ROOT.tar.zip
!tar -xf  ROOT.tar
!apt-get install git dpkg-dev cmake g++ gcc binutils libx11-dev libxpm-dev libxft-dev libxext-dev tar gfortran subversion
```

The first line of this code download a tar file containting the ROOT folder from my github repository. The second line unzip the tar file. The third line extract the ROOT folder from the tar file. The last line install the required packages to build ROOT.

Then I added the following code to the second cell:

```python
import sys
sys.path.append("/content/root_build/")
sys.path.append("/content/root_build/bin/")
sys.path.append("/content/root_build/include/")
sys.path.append("/content/root_build/lib/")
import ctypes
ctypes.cdll.LoadLibrary('/content/root_build/lib//libCore.so')
ctypes.cdll.LoadLibrary('/content/root_build/lib//libThread.so')
ctypes.cdll.LoadLibrary('/content/root_build/lib//libTreePlayer.so')
```

The first line of this code add the ROOT folder to the python path. The second line add the bin folder to the python path. The third line add the include folder to the python path. The last line add the lib folder to the python path. Then it load the ROOT libraries.

Now we are ready to use ROOT in colab. I added the following code to the third cell:

```python
import ROOT

h = ROOT.TH1F("gauss","Example histogram",100,-4,4)
h.FillRandom("gaus")
c = ROOT.TCanvas("myCanvasName","The Canvas Title",800,600)
h.Draw()
c.Draw()
```

You can see the output of this code in the following image:

![ROOT example output](/assets/images/posts/root-colab/hist.png)




Also you can find example notebook on Colab [here](https://colab.research.google.com/drive/1L7jx5NJS3mPDoX1jw9Fj52_0zb1aLYIK?usp=sharing) that you can use directly.