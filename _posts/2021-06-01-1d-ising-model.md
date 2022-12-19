---
published: true
layout: post
title: "Monte Carlo Simulation of 1d Ising Model"
description: "Monte Carlo Simulation of a simple 1d Ising Model "
comments: true
keywords: "Monte Carlo, Ising Model, Python, 1d"
---

- [Introduction](#introduction)
- [Monte Carlo Simulation](#monte-carlo-simulation)
- [1D Ising Model Simulation](#1d-ising-model-simulation)
- [Ising Model Animation](#ising-model-animation)
- [Magnetization and heat capacity](#magnetization-and-heat-capacity)
- [Conclusion](#conclusion)



## Introduction

The Ising model is a mathematical model used to describe the behavior of a system of ferromagnetic materials. It was first proposed by physicist Wilhelm Lenz in 1920 and has since been widely used to study phase transitions and critical phenomena in statistical mechanics.

One way to simulate the 1D Ising model in Python is through the use of the Monte Carlo method. This involves generating a large number of random configurations of the system and using these to estimate the thermodynamic properties of the system.

## Monte Carlo Simulation

Monte Carlo simulation is a statistical method used to model the behavior of a system by generating a large number of random configurations and using these to estimate the probability of certain outcomes. It is commonly used in many fields to analyze complex systems and make predictions about their behavior.

To illustrate, consider the problem of estimating the value of π using a Monte Carlo simulation in Python. To do this, we can generate a large number of random points within a square and count the number that fall within a quarter circle inscribed within the square. The probability that a given point will fall within the quarter circle is equal to the ratio of the area of the quarter circle to the area of the square, which is equal to π/4. By dividing the number of points that fall within the quarter circle by the total number of points, we can estimate the value of π. This simple example illustrates the basic idea behind Monte Carlo simulation, which is to use random sampling to estimate the probability of certain outcomes

Here is an example of how this could be implemented in Python:

``` python
import random

# Number of points to generate

n = 100000

# Counter for points within the quarter circle

count = 0

# Generate the points

for i in range(n):
    x = random.uniform(-1, 1)
    y = random.uniform(-1, 1)
    
    # Check if the point is within the quarter circle

    if x**2 + y**2 <= 1:
        count += 1

# Calculate the estimated value of pi

pi_estimate = 4 * count / n
print(pi_estimate)


```



The first line imports the `random` module, which provides functions for generating random numbers. The next line sets the number of points to generate to 100,000. The following line initializes a counter for the number of points that fall within the quarter circle.

The main part of the code is a `for` loop that iterates over a range of n values. Within the loop, the code generates two random numbers between -1 and 1 using the `random.uniform()` function and assigns them to the variables `x` and `y`. These represent the coordinates of a point within the square.

The code then checks whether the point is within the quarter circle by checking if `x**2 + y**2 <= 1`. If this condition is true, the counter is incremented. This process is repeated for `n` iterations, generating `n` random points and counting the number that fall within the quarter circle.

Finally, the code calculates the estimated value of `π` by dividing the count by `n` and multiplying by 4. The result is printed to the console using the `print()` function.


## 1D Ising Model Simulation

To begin, we will need to import the necessary libraries and define some key variables. The first thing we will need is a way to store the state of the system. We can do this using a list, where each element represents the spin of a particular particle in the system. For simplicity, we will assume that all particles have the same spin.


``` python
import random
import math
import numpy as np

def generate_random_configuration(n, p):

    spins = []
    for i in range(n):
        if random.random() < p:
            spins.append(-1)
        else:
            spins.append(1)
    return spins


```

Next, we will need to define the energy of each configuration. In the 1D Ising model, the energy of a configuration is given by the sum of the interactions between neighboring particles. For a given configuration, we can calculate the energy by looping through the list of spins and adding up the interactions between each pair of neighboring spins.

Once we have a way to store the state of the system and calculate the energy, we can start using the Monte Carlo method to simulate the system. The basic idea is to generate a large number of random configurations and use these to estimate the thermodynamic properties of the system. To do this, we will need to define a function that generates a random configuration of the system. This can be done using a simple loop that flips the spin of each particle with a certain probability.


``` python
def calculate_energy(spins):
    
    n = len(spins)
    energy = 0
    for i in range(n-1):
        energy += -spins[i] * spins[i+1]
    return energy

```

Next, we will need to define a function that performs a single Monte Carlo step. This involves generating a random configuration, calculating the energy of the configuration, and comparing it to the energy of the current configuration. If the new configuration has a lower energy, we accept it. If the new configuration has a higher energy, we accept it with a probability given by the Boltzmann factor.

``` python
def monte_carlo_step(spins, temperature):

    n = len(spins)

    new_spins = spins.copy()
    
    # Choose a random particle

    i = random.randint(0, n-1)
    
    # Calculate the change in energy due to flipping the spin

    delta_energy = 2 * spins[i] * (spins[(i-1)%n] + spins[(i+1)%n])
    
    # Flip the spin with a probability given by the Boltzmann factor

    if delta_energy < 0 or random.random() < math.exp(-delta_energy/temperature):
        new_spins[i] = -spins[i]
    return new_spins
```

Finally, we will need to define a function that runs the Monte Carlo simulation for a specified number of steps. This can be done by calling the Monte Carlo step function repeatedly until the desired number of steps has been reached.


``` python
def monte_carlo_simulation(n, temperature, steps):

    # Generate a random initial configuration

    spins = generate_random_configuration(n, 0.5)
    
    # Perform the Monte Carlo steps

    for i in range(steps):
        spins = monte_carlo_step(spins, temperature)
        
    return spins
```

Now using the implemented function, we can run the simulation with n=10 particles, temperature=1, and steps=1000 and print the final configuration.

``` python
final_configuration = monte_carlo_simulation(10, 1, 1000)
print(final_configuration)
```

The output will be a list of 10 spins , where each element is 1 or -1. The spins will be arranged in a way that minimizes the energy of the system. This is because the Monte Carlo method is designed to find the configuration of the system that minimizes the energy.

``` shell
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
```

To summerize the code: 

- `generate_random_configuration()`: This function generates a random configuration of the system by randomly flipping the spin of each particle with a certain probability. It takes two arguments: n, the number of particles in the system, and p, the probability of flipping the spin of a particle. It returns a list of n spins, where each element is 1 or -1.

- `calculate_energy()`: This function calculates the energy of a given configuration. It takes a single argument, spins, the configuration of the system. It returns the energy of the configuration as a float.

- `monte_carlo_step()`: This function performs a single Monte Carlo step. It generates a new configuration by randomly selecting a particle and flipping its spin. The new configuration is accepted with a probability given by the Boltzmann factor. It takes two arguments: `spins`, the current configuration of the system, and `temperature`, the temperature of the system. It returns the new configuration as a list.

- `monte_carlo_simulation()`: This function runs the Monte Carlo simulation for a specified number of steps. It generates a random initial configuration and then repeatedly calls the `monte_carlo_step()` function until the desired number of steps has been reached. It takes three arguments: `n`, `the number of particles in the system`, `temperature`, the temperature of the system, and `steps`, the number of Monte Carlo steps to perform. It returns the final configuration as a list.

Finally, the code calls the `monte_carlo_simulation()` function with `n=10`, `temperature=1`, and `steps=1000` to run the simulation with 10 particles, a temperature of 1, and 1000 Monte Carlo steps. It prints the final configuration to the console using the `print()` function.


## Ising Model Animation

To animate the 1D Ising model , we can use a library that provides tools for creating animations, such as `matplotlib`, `mayavi`, or `plotly`.  I'm going to use the famous `matplotlib` to do this. First we need to import the library

``` python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
```

Now we set up the figure and axes

``` python
fig, ax = plt.subplots()
```


After that we need to Initialize the data for the plot using the generate_random_configuration() function defined in the previous section. It creates a scatter plot using the scatter function, with the spins as the `y-coordinates` and the particle indices as the `x-coordinates`

``` python
n = 10
spins = generate_random_configuration(n, 0.5)
scatter = ax.scatter(range(n), spins, c=spins, cmap='coolwarm')
```

We then create an `update()` function, which updates the plot at each frame of the animation. This function performs a single Monte Carlo step using the `monte_carlo_step()` function and then updates the scatter plot with the new spin values using the `set_offsets()` and `set_array()` methods. I was getting `AttributeError: 'list' object has no attribute 'ndim'` error because the `scatter.set_array()` method expects a numpy array as its argument, but I was passing it a Python list. This causes the `ndim` attribute to be accessed on the list object, which does not exist and raises the AttributeError. To fix this, I  converted the spins list to a numpy array using the `np.array()` function before passing it to the `set_array()` method:





``` python
# Function to update the plot at each frame

def update(i):
    global spins
    spins = monte_carlo_step(spins, 1)
    scatter.set_offsets(np.c_[range(n), spins])
    scatter.set_array(np.array(spins))
    return scatter
```

Finally, we create the animation using the `FuncAnimation()` function, specifying the figure, the update function, the number of frames, and the blit parameter. It then displays the animation using the `show()` function.

``` python
# Create the animation

anim = animation.FuncAnimation(fig, update, frames=100, blit=False)
plt.show()
```


PS: If you are using Google colab, it will not show you the animation. The easiest way is to 'jshtml' to display matplotlib animation. You need to add the following line of code before the animation code:

``` python
from matplotlib import rc
rc('animation', html='jshtml')
```

Then, just type your animation object. It will display itself, i.e `anim` in this case.

``` python
anim
```

Alternatively, you can use `anim.to_html5_video` from `IPython.display.HTML` using the following code:

``` python
from IPython.display import HTML
HTML(anim.to_html5_video())
```

The outout will be like this:

<iframe width="420" height="315" src="/assets/images/posts/1d-ising/animation.mp4" frameborder="0" allowfullscreen></iframe>

## Magnetization and heat capacity

To calculate the magnetization and heat capacity of the 1D Ising model, we can define two additional functions in our code:

``` python
def calculate_magnetization(spins):
    """Calculate the magnetization of a spin configuration."""
    return np.mean(spins)
```

The `calculate_magnetization()` function takes a spin configuration (a list or array of `1s` and `-1s`) and returns the mean value of the spins, which is a measure of the overall magnetization of the system.



and for the heat capacity:

``` python
def calculate_heat_capacity(spins, temperature):
    """Calculate the heat capacity of a spin configuration at a given temperature."""
    energy = calculate_energy(spins)
    magnetization = calculate_magnetization(spins)
    heat_capacity = (energy**2 + magnetization**2) / temperature**2
    return heat_capacity
```

The `calculate_heat_capacity()` function takes a spin configuration and a temperature, and returns the heat capacity of the system. It calculates the energy and magnetization of the system using the `calculate_energy()` and `calculate_magnetization()` functions defined earlier, and then uses these values to compute the heat capacity using the formula: `C = (E^2 + M^2) / T^2` or `heat_capacity = (energy2 + magnetization2) / temperature**2`.

We can now use these functions to calculate the magnetization and heat capacity of the system at each step of the animation by calling them in the `update()` function:


``` python
def update(i):
    global spins
    spins = monte_carlo_step(spins, 1)
    magnetization = calculate_magnetization(spins)
    heat_capacity = calculate_heat_capacity(spins, temperature)
    scatter.set_offsets(np.c_[range(n), spins])
    scatter.set_array(np.array(spins))
    return scatter
```




We can then use these values to update a plot of the magnetization and heat capacity as the animation progresses. You can create these plots using additional axes on the same figure, or you can create separate figures for each plot. We need to write our animation as a function of temperature. This can be done as follows:

``` python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import numpy as np

def plot_ising_model(n, temperature):

    # Set up the figure and axes

    fig, (ax1, ax2) = plt.subplots(2, 1, sharex=True)
    ax1.set_title('1D Ising Model')
    ax1.set_ylabel('Magnetization')
    ax2.set_ylabel('Heat Capacity')

    # Initialize the data

    spins = generate_random_configuration(n, 0.5)
    scatter = ax1.scatter(range(n), spins, c=spins, cmap='coolwarm')

    # Set up the magnetization and heat capacity plots

    magnetization_line, = ax1.plot([], [], color='blue')
    heat_capacity_line, = ax2.plot([], [], color='red')

    # Function to update the plots at each frame

    def update(i):
        global spins
        spins = monte_carlo_step(spins, temperature)
        magnetization = calculate_magnetization(spins)
        heat_capacity = calculate_heat_capacity(spins, temperature)
        scatter.set_offsets(np.c_[range(n), spins])
        scatter.set_array(np.array(spins))
        magnetization_line.set_data(range(i), magnetization)
        heat_capacity_line.set_data(range(i), heat_capacity)
        return scatter, magnetization_line, heat_capacity_line

    # Create the animation

    anim = animation.FuncAnimation(fig, update, frames=100, blit=False)
    #plt.show()
```

The `plot_ising_model()` function takes the number of spins and the temperature as arguments, and creates a figure with two axes. It then creates a scatter plot of the spin configuration, and two line plots for the magnetization and heat capacity. It then defines an `update()` function that updates the scatter plot and the two line plots at each frame. Finally, it creates the animation and displays it using the `show()` function. To use this function, we can call it with different values of the temperature:

``` python
# Set the number of particles and the temperature (in units of J/k)

n = 10
temperature = 100

# Plot the 1D Ising model

plot_ising_model(n, temperature)
```


## Conclusion
Ising model is a simple model that can be used to study the phase transition in a system. In this post, we have studied the 1D Ising model using the Monte Carlo method. We have also seen how to create an animation of the system using matplotlib. We have also seen how to calculate the magnetization and heat capacity of the system at each step of the animation. In the next post, we will study the 2D Ising model.
