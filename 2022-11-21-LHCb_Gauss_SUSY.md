---
published: false
---
---
layout: post
title: "How to modify particles in LHCb Gauss"
description: "Modifying the properties of particles in ParticleTable.txt"
comments: true
keywords: "LHCb, Gauss, LHC, CERN, particles, SUSY, SuperSymmetry"
---

I am currently working on a new analysis at LHCb. I'm trying to invistigate the possibility of long lived stau decaying to a gravitino and a tau. This is a scenario predicted by Gauge Mediated Supersymmetry Breaking (GMSB) SuperSymmetric (SUSY) model. The  very light gravitino in this model will be the lightest particle with stau being next leading particle (NLP). There is little interest shown in the past into probing phase space so this leaves a gap in exploration. LHCb detector provides a unique opportunity because of its long tracker. This would allow us to probe higher lifetimes with potential high sensitivity. As this a very new signal and SUSY is not something established into LHCb software yet, I found myself dealing with a lot of things but also I enjou learning. I will try to write about some of these things as a way of documenting things here.

Gauss, the software package used for simulation inside LHCb provides information about particles through simple txt file stored in DDDB package under `param/ParticleTable.txt`. Gauss scrape the following information from the file. 

```
Particle GeantID PDGID CHARGE MASS(GeV) LIFETIME(s) EVTGENNAME PYTHIAID MAXWIDTH
```

For example, for Kaon+ it would be 
```
K*_2(1430)0         153         315  0.0        0.100000   1.0e-10                    K_2*0         315        0.0
```

For all SUSY particles and most of non standard model particles you will [find](https://gitlab.cern.ch/lhcb-conddb/DDDB/-/blob/master/param/ParticleTable.txt) default values that is not right (except of IDs)

For example for stau and its anti particles

``` bash
 ~tau_1-               879     1000015  -1.0    500.00000000      0.000000e+00                   unknown     1000015      0.00000000
 ~tau_1+               880    -1000015   1.0    500.00000000      0.000000e+00                   unknown    -1000015      0.00000000 
 ```
 
I wanted a way to specify the Mass and lifetime of generated staus because I wanted to generate different samples for my study. One way to do that was to play with width and mass values inside the `SLHA` file that I generated from `IsaJet` which I found some people at CMS do. I didn't like this solution as there are some unintended consequences for that. Forunetnley there is a method called `ParticlePropertySvc` the I found mention about in one of the rare occasion where I find a [Twiki page](https://twiki.cern.ch/twiki/bin/view/LHCb/FAQ/LHCbFAQ#How_do_I_modify_the_entries_of_t) useful. 

So I wanted to generate staus with long life time enough to be in order of meters. If I'm taking the maximum value of 10 m (length of LHCb tracker) this means that the lifetime would be `~ 3.4e08` or something in this order for lower values.  

The code to use it is pretty simple. I have to change the mass and lifetime (be careful with units). I give Gravitino very high lifetime because I want it to be considered stable. 

``` python
from Configurables import LHCb__ParticlePropertySvc
LHCb__ParticlePropertySvc().Particles += [
      "~tau_1- 887  1000015  -1.0 %e %e unknown  1000015 0.00000000" % (100, 3.34e-8),
      "~Gravitino 892  1000039  0.0 %e %e unknown  1000039 0.00000000" % (0, 1e08)
    ]
```    

Now when you run gauss you should get updates from ParticlePropertySvc in the logs. You will see something like 

```
LHCb::ParticlePropert...SUCCESS  New/updated particles (from "Particles" property)
```

Also it is useful to print the updated information. You should get something like that

``` bash
------------------------------------------------------------------------------------------------------------------------------------------------------------------
 | #    |        Name       |     PdgID    |   Q  |        Mass       |    (c*)Tau/Gamma  |  MaxWidth  |        EvtGen        |  PythiaID  |     Antiparticle     |
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------
 | 792  | ~tau_1-           |      1000015 |  -1  |           100 GeV |     10.013068 m   |      0     |        unknown       |   1000015  |        ~tau_1+       |
 | 804  | ~Gravitino        |      1000039 |   0  |             0 eV  |        stable     |      0     |        unknown       |   1000039  |        self-cc       |
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------
 ```
 
 I think we will also need to do the same with the anti-particle `~tau_1+`. 
