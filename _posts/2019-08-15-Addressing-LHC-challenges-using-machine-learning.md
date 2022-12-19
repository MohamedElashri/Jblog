---
layout: post
title: "Addressing LHC challenges using machine learning"
description: "My experience with Addressing LHC challenges using machine learning coursera course."
keywords: "machine learning, physics, LHC, coursera"
comments: true
---

-----------------------


A few months ago, I completed one of the most challenging courses I've ever taken in my three years of studying MOOCs. It was the final course in the Advanced Machine Learning specialization offered by the National Research University Higher School of Economics, which provides excellent online classes that put you on the same academic level as in-person classes. I have completed most of the seven courses in this specialization, and I was in the process of finishing the last two when I decided to write about this particular course.


I started this course with a background in particle physics. I graduated from a high energy physics program, and I did a thesis on upgrading the CMS experiment detector, which is one of the largest LHC experiments. This work gave me insight into the physics behind the problems we are trying to solve with ML. However, you don't need to have this knowledge; the introduction videos provide enough information for those outside the field to complete the course. I have a good amount of experience with machine learning, but even with this, I had a lot of difficulties completing this course. Some of these problems were due to the fact that I was one of very few people taking this new course at the time, and there were many bugs in the assignments and unclear requirements. The most annoying issue was the compatibility of some libraries that were upgraded without the course instructors updating the notebooks regularly, but this has now been fixed and is no longer a problem.

The course gave us insight into how we can use ML models to do physics analysis on real data and produce answers. You can see this in the first week's assignment, which I have linked to (although you should try to tackle it yourself first)[here](https://github.com/MohamedElashri/Hadron-Collider-ML/blob/master/Z_Boson_mass_measurement.ipynb). The course then uses the most powerful application of ML, classification, for particle identification, which is one of the most important tasks in particle physics. Since we have a lot of particles passing through each part of our detector, we want to know which went where and how we can reconstruct their tracks, measure their momentum and energy, etc. Fortunately, this task is suitable for ML, and we can train a classifier to give us answers to this question, using our physical and experimental parameters as features (See how enjoyable this work [here](https://github.com/MohamedElashri/Hadron-Collider-ML/blob/master/Particle_identification.ipynb)).



Week 3 of the course introduces the concept of using Monte Carlo simulation and why it is essential in particle physics, and how we can use ML to compare real and simulated data to search for new physics signatures (you can take a look at this ([here](https://github.com/MohamedElashri/Hadron-Collider-ML/blob/master/Search_for_rare_decay.ipynb)). I have to say that week 3 was probably the hardest, but I enjoyed it the most. In week 4, we applied what we had learned so far to search for dark matter signatures using simulations from a new proposed experiment that is expected to start collecting data in 2025 (hopefully) The code is . You can see this work [here](https://github.com/MohamedElashri/Hadron-Collider-ML/blob/master/Searching_for_electromagnetic_showers.ipynb). Finally, hardware work in particle physics is just as important as data analysis, and it also requires a lot of programming (I'm also partially working on the NOvA experiment hardware). So, taking the time and effort to play with optimizing a simple tracking system using Bayesian optimization with Gaussian processes was a great final problem to tackle using ML in this course (see [here](https://github.com/MohamedElashri/Hadron-Collider-ML/blob/master/Detector_Optimization.ipynb), and my complete GitHub repository is [here](https://github.com/MohamedElashri/Hadron-Collider-ML)).

In the end, I would say that this specialization follows the Russian learning method of "throwing them in the sea" to teach them how to swim. Some may argue against this approach, but I like it, especially when it combines two things I love: particle physics and machine learning.

I hope this might help someone interested in joining this field, as ML will be a valuable tool that we will need in the coming decades of our search for new physics and the expansion of our knowledge about the physical world.


