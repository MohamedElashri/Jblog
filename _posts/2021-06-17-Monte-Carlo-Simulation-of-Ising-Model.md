---
published: true
layout: post
title: "Monte Carlo Simulation of Ising Model"
description: "Monte Carlo Simulation of Ising Model using Metropolis algorithm Python and Numba"
comments: true
keywords: "Monte Carlo, Ising Model, Python, Numba, Metropolis Algorithm"
---

## Introduction
During my first year PhD at UC, I get the chance to take statistical mechanics class from a condensed matter physicist. It was a new experience when he focused too much on different side that I did not explore before. The final project in this class was about using Monte-Carlo method to simulate the Ising model. Ising model is a model of a system that undergoes a phase transition. There are many reasons why this model is interesting when it is not representing any actual physical systems.

1. It can be solved exactly, We can calculate the thermodynamical quantities and interpret them. In another can calculate the partition function and put it in an exact form.
2. Ising model exhibits symmetry breaking in low-temperature phase.
3. It is a simple model that can be used to understand the physics of phase transition.
4. The Ising model is simple, yet it can be applied to many physical systems. This gives us a feature about universality, a feature that tells us that in such case the same theory applies to all sorts of different phase transitions.

## The Ising Model
We can think of Ising model as a mathematical model where everything consists of small squares inside a bigger one. It is usually called lattices where we then have a big lattice of sites (small boxes) where each site can be in one of two states. Those states as we will see will refer to the spin state of the electron inside this site. Each site will be labeled by index ii. The Hamiltonian for the Ising model can be written as

\[ \mathcal{H}=-J \sum_{\langle i j\rangle} S_i S_j \]


![Lattice square representation of Ising model, https://stanford.edu/~jeffjar/statmech/intro4.html](/assets/images/posts/ising/ising.jpg){: .center-image }


- Spin \( S_i \) has value of +1 or -1
- \( \langle i j\rangle \) implying that we are interested in nearest-neighbor interaction only
- \( J \) is the interaction strength where \( J>0 \) assuming that strength is positive. 

There are many systems that can be represented using this model like "lattice gas" with each of the sites is the possible location of a particle;  means that site is empty and  means that site is occupied by a particle.

This system will undergo a 2nd order phase transition at some temperature \(T=T_c \) which is called critical temperature. For cases when \(T < T_c \), the system magnetizes, and the state is called the ferromagnetic state which is an ordered state. For cases when \(T > T_c \)​,  the system is  disordered or in a paramagnetic state. We can define the order parameter for this system to be the average magnetization \(m \).

\[ m=\frac{\langle S\rangle}{N} \]

This parameter is used to distinguish between the two phases that this system can have. It is zero when we are in disordered (paramagnetic) state and takes a non-zero value in ordered (ferromagnetic) state.

## Monte-Carlo Simulation

In this section, I'm going through the details of implementing the Monte-Carlo (MC) method for our Ising model. I apply the MC methods using Metropolis Algorithm to Ising model and extract physical parameters (Energy, Specific heat and Magnetization).

### Physical Model
Lattice is a periodical structure of points that align one by one. 2D lattice can be plotted as:

```
* * * * * * * *   
* * * * * * * * 
* * * * * * * *
* * * * * * * *
* * * * * * * *
```

The points in lattice are called lattice points, nearest lattice points of point ^ are those lattice points denoted by (*) shown in the graph below:

```
* * *(*)* * * *
* *(*)^(*)* * *
* * *(*)* * * *
* * * * * * * *
```

Each lattice point is denoted by a number i in the Hamiltonian.

The expression for the Energy of the total system is 

\[ H=-J \sum_{i=0}^{N-1} \sum_{j=0}^{N-1}\left(s_{i, j} s_{i, j+1}+s_{i, j} s_{i+1, j}\right) \]

```
* * * * * * * * 
* * * * * * * *
* * * * * * * * <-the i-th lattice point
* * * * * * * *
* * * * * * * *
```
Periodical structure means that lattice point at(1,1) is the same as that at(1,9) if the lattice is 5 x 8. For example (1,1)<=>(6,1), (2,3)<=>(2,11). A 2D lattice can be any \( N_x \) by \(N_y \). The location \( (x,y) \) here is another denotion of lattice point that is fundamentally same as i-th lattice point denotation above.

```
* * * * * * * * 4
* * * * * * * * 3
* * * * * * * * 2
* * * * * * * * 1
1 2 3 4 5 6 7 8 
```

### Python Implementation

Algorithm

    1. Prepare some initial configurations of N spins.
    2. Flip spin of a lattice site chosen randomly
    3. Calculate the change in energy due to that
    4. If this change is negative, accept such move. If change is positive, accept it with probability exp^{-dE/kT}
    5. Repeat 2-4.
    6. Calculate Other parameters and plot them


#### Python code for Ising model

```python
#----------------------------------------------------------------------
##  Import needed python libraries 
#----------------------------------------------------------------------
import matplotlib.pyplot as plt
from numba import jit # wonderful optimization compiler. 
import numpy as np  # we can't work in python without that in physics (maybe we can if we are not lazy enough)
import random # we are doing MC simulation after all!!
import time # for time estimation 
from tqdm import tqdm # fancy progress bars for loops , use this line if working with .py script
#from tqdm.notebook import tqdm # use this if working with jupyter notebook to avoid printing a new line for each iteration
from rich.progress import track # we can use  rich library.change "tqdm" later to "track"
```

```python
# Define parameters 

B = 0; # Magnetic field strength
L = 50; # Lattice size (width)
s = np.random.choice([1,-1],size=(L,L)) # Begin with random spin sites with values (+1 or -1) for up or down spins. 
n= 1000 * L**2 # number of MC sweeps 
Temperature = np.arange(1.6,3.25,0.01) # Initlaize temperature range, takes form np.arange(start,stop,step)
```

```python
# Define functions to compute energy and magnetization

'''
Energy of the lattice calculations. 
The energy here is simply the sum of interactions between spins divided by the total number of spins
'''
@jit(nopython=True, cache=True) # wonderful jit optimization compiler in its high performance mode
def calcE(s):
    E = 0
    for i in range(L):
        for j in range(L):
            E += -dE(s,i,j)/2
    return E/L**2
    
 '''
Calculate the Magnetization of a given configuration
Magnetization is the sum of all spins divided by the total number of spins
'''
@jit(nopython=True, cache=True) # wonderful jit optimization compiler in its high performance mode
def calcM(s):
    m = np.abs(s.sum())
    return m/L**2
```

```python
# Calculate interaction energy between spins. Assume periodic boundaries
# Interaction energy will be the difference in energy due to flipping spin i,j 
# (Example: 2*spin_value*neighboring_spins)
@jit(nopython=True, cache=True) # wonderful jit optimization compiler in its high performance mode
def dE(s,i,j): # change in energy function
    #top
    if i == 0:
        t = s[L-1,j]  # periodic boundary (top)
    else:
        t = s[i-1,j]
    #bottom
    if i == L-1:
        b = s[0,j]  # periodic boundary (bottom)
    else:
        b = s[i+1,j]
    #left
    if j == 0:
        l = s[i,L-1]  # periodic boundary (left)
    else:
        l = s[i,j-1]
    #right
    if j == L-1:
        r = s[i,0]  # periodic boundary  (right)
    else:
        r = s[i,j+1]
    return 2*s[i,j]*(t+b+r+l)  # difference in energy is i,j is flipped
```

```python
# Monte-carlo sweep implementation
@jit(nopython=True, cache=True) # wonderful jit optimization compiler in its high performance mode
def mc(s,Temp,n):   
    for m in range(n):
        i = random.randrange(L)  # choose random row
        j = random.randrange(L)  # choose random column
        ediff = dE(s,i,j)
        if ediff <= 0: # if the change in energy is negative
            s[i,j] = -s[i,j]  # accept move and flip spin
        elif random.random() < np.exp(-ediff/Temp): # if not accept it with probability exp^{-dU/kT}
            s[i,j] = -s[i,j]
    return s
```

```python
# Compute physical quantities
@jit(nopython=True, cache=True) # wonderful jit optimization compiler in its high performance mode
def physics(s,T,n):
    En = 0
    En_sq = 0
    Mg = 0
    Mg_sq = 0
    for p in range(n):
        s = mc(s,T,1)
        E = calcE(s)
        M = calcM(s)
        En += E
        Mg += M
        En_sq += E*E
        Mg_sq += M*M
    En_avg = En/n
    mag = Mg/n
    CV = (En_sq/n-(En/n)**2)/(T**2)
    return En_avg, mag, CV
```

```python
# Inititalize magnetization, average energy and heat capacity
mag = np.zeros(len(Temperature))
En_avg = np.zeros(len(Temperature))
CV = np.zeros(len(Temperature))

start = time.time()
```

``` python
# Simulate at particular temperatures (T) and compute physical quantities (Energy, heat capacity and magnetization)
for ind, T in enumerate(track(Temperature)):
    # Sweeps spins
    s = mc(s,T,n)
    # Compute physical quanitites with 1000 sweeps per spin at temperature T
    En_avg[ind], mag[ind], CV[ind] = physics(s,T,n)
end = time.time()
print("Time it took in seconds is = %s" % (end - start))

time = (end - start)/60
print('It took ' + str(time) + ' minutes to execute the code')
```

```python
#----------------------------------------------------------------------
#  Plotting area
#----------------------------------------------------------------------


f = plt.figure(figsize=(18, 10)); # plot the calculated values  
sp =  f.add_subplot(2, 2, 1 );
plt.plot(Temperature, En_avg, marker='.', color='IndianRed')
plt.xlabel("Temperature (T)", fontsize=20);
plt.ylabel("Energy ", fontsize=20);         plt.axis('tight');

sp =  f.add_subplot(2, 2, 2 );
plt.plot(Temperature, abs(mag), marker='.', color='RoyalBlue')
plt.xlabel("Temperature (T)", fontsize=20); 
plt.ylabel("Magnetization ", fontsize=20);   plt.axis('tight');

sp =  f.add_subplot(2, 2, 3 );
plt.plot(Temperature, CV, marker='.', color='IndianRed')
plt.xlabel("Temperature (T)", fontsize=20);  
plt.ylabel("Specific Heat ", fontsize=20);   plt.axis('tight');   

plt.subplots_adjust(0.12, 0.11, 0.90, 0.81, 0.26, 0.56)
plt.suptitle("Simulation of 2D Ising Model by Metropolis Algorithm\n" + "Lattice Dimension:" + str(L) + "X" + str(
    L) + "\n" + "External Magnetic Field(B)=" + str(B) + "\n" + "Metropolis Step=" + str(n))
plt.show() # function to show the plots
```

## Results

These are plots of the physical quantities for different MC steps.

![Simulation results for 50*50 lattice with 500000 steps](/assets/images/posts/ising/plot_1.png){: .center-image }

![Simulation results for 50*50 lattice with 250000 steps](/assets/images/posts/ising/plot_2.png){: .center-image }


## Reproduction

You can run the Jupyter Notebook provided on Colab directly, or you can download from my project [GitHub repository](https://github.com/MohamedElashri/IsingModel) and run locally. Also, there is python script that you can run. I'm using Numba cache so that it produces cache folder (specific to machine CPU and configuration) so that you will need to produce yours by running it for one time and subsequent runs will be about twice faster.

To run the script

1. clone the repository
``` bash
git clone https://github.com/MohamedElashri/IsingModel
```
2. run the script
``` bash
python IsingModel/IsingModel.py
```


## Appendix I

I spent many nights working on this work, most of time I needed to optimize my code, I even tried to move to Matlab (last time I used it was like 5 years ago). But I learned a nice thing from my desire to optimize code speed. It is the usage of Numba’s JIT compiler. Read more about that [here](https://web.archive.org/web/20210802225233/http://melashri.net/url/b). I also instead of using multiple nested loops I dragged all these into just one. Imagine running 50x50 lattice simulation in my older codes for hours (one took 6 hours) vs 15 minutes for the current script. (On my Mac m1 Machine). Also, I made the code available on colab and can be accessed [here](https://web.archive.org/web/20210802225233/http://melashri.net/url/c) (without many comments).

## Appendix II
I have used this codebase as a benchmark test for python releases. I plan on maintain this test (available as [repo](https://github.com/MohamedElashri/python_bench_ising) on github). Initialy I used numpy-1.23.4 for all different version of python used in the benchmark. This is the latest version of numpy. I got the following results.


### Results

The benchmarks are executed on Macbook pro Apple Silicon M1 version. The python version are installed using `homebrew`

The arguments of the script are L(length of the lattice),  n(number of Monte Carlo cycles). 
#### python 3.11

```bash
time python3.11 ising.py 10 10000
Time it took in seconds is = 109.2954261302948
python3.11 ising.py 10 10000  110.82s user 0.79s system 100% cpu 1:51.38 total
```

#### python 3.10 

```bash
time python3.10 ising.py 10 10000
Time it took in seconds is = 123.83638191223145
python3.10 ising.py 10 10000  124.80s user 0.69s system 101% cpu 2:04.06 total
```

#### python 3.9


```bash
time python3.9 ising.py 10 10000
Time it took in seconds is = 123.56476402282715
python3.9 ising.py 10 10000  123.94s user 1.08s system 101% cpu 2:03.67 total
```

#### python 3.8

```bash
time python3.8 ising.py 10 10000
Time it took in seconds is = 135.1741383075714
python3.8 ising.py 10 10000  137.05s user 1.00s system 100% cpu 2:17.24 total
```

#### Other versions 

Python started supporting apple silicon starting from `python3.8` and I try to make things as consistent as possible. Not to mention that `numpy` optimization for different version might be unaccounted difference. 
