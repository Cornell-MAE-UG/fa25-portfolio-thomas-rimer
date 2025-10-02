---
layout: project
title: Weight Lifting Mechanism
description: In-Class Assignment for Statics
technologies: [Pencil and Paper]
image: /assets/images/linear_actuator/tolo.png
---

# Overview

In ENGRD 2020, we were asked to come up with a design for the following prompt:

*Given a 2D design space of 150cm long and 50cm tall, a rigid bar of a fixed length (your choice), 3 pin supports of which two need to be mounted on the ground and a linear actuator (pick from this [online](https://www.tolomatic.com/wp-content/uploads/2022/05/2700-4000_29_IMA_cat.pdf) catalog, use max force values only), design a frame/mechanism to lift the maximum possible weight to the highest possible height. Assume all the supports and bar/actuator are rigid.*

## 1) Establishing a Metric
This problem asks us to lift the maximum possible weight to the maximum possible height. These are two seperate variables, so to judge the performance of a certain design, we need a unified metrix which takes both of these in to account.

I propose we use gravitational potential energy, mgh, which is proportional to both height and mass of the lifted object (multiplied by ~10). This favors height and mass equally (i.e. lifting a mass to a height is the same as lifting double the mass half as high). It also has a real, physical meaning (work done on object) which could be used for future problems. 

## 2) Rough Proposed Deisgn 
I propose the following design which I will then optimize later.

A long rod is pinned in the bottom left corner and extends to the right side of the box. On the right end of the rod is the mass. The linear actuator is pinned to the bottom of the box at a certain horizontal location x_l. The other end of the linear actuator is pinned to the rod at a position x_r units along the rod. Below is a drawing of the setup.

![Rough drawing of mechanism]({{ "assets/images/linear_actuator/mech_1.png" | relative_url }})

### 2.1) Assumptions
- Rod is assumed to be stiff
- The rod can be pinned exactly at its end (i.e. it doesn't need extra material on its end)
- The size/volume of the mass is negligble (i.e. it's a point mass)
- The rod is massless
- There is no friction in the pins

### 2.2) Variables
#### Independent Variables
- x_r: pin position of the linear actuator along the rod
- θ: pin position of the linear actuator to the ground
- Linear actuator of choice
    - L_ret: length of retracted linear actuator
    - L_ext: length of extended linear actuator
    - F: force of linar actuator

#### Dependent Variables
- Δmgh: change in gravitational potential energy (from starting to final configuration)

## 3) The Model
To solve this optimization problem, I graphed the solution in 3D using Desmos. By also fencing off regions of the domain prohibited by the problem, I was able to determine valid starting conditions and manually identify the local maximum. Below is a step by step explanation of the model:

### 3.1) Inputs
The model has 5 inputs
- x_0 (plotted along x axis in plots)
    - Horizontal position of the pin holding the linear actuator to the ground
    - Varied between 0 and 1.5 (fully left or fully right of the box) (bounds of the x axis on all plots)
    - Directly tied to x axis on the plots
    - Units: meters
- θ_0 (plotted along the y axis in plots)
    - Angle between the ground and the linear actuator, as measured from the +x axis CCW, when the linear actuator is fully retracted
    - Varied between 0 and π (horizontal facing right to horizontal facing left) (bounds of the y axis on all plots)
    - Directly tied to y axis on the plots
    - Units: radians
- L_ret
    - Length of the linear actuator in its fully retracted state
    - Units: meters
- L_ext
    - Length of the linear actuator in its fully extended state
    - Units: meters
- F_max
    - Force exterted by the linear actuator
    - Units: newtons

### 3.2) Intermediate Calculations
First, the coordinates (b_0x, b_0y) of the pin connecting the linear actuator to the rod in the retracted state was calculated by adding the linear actuator vector to the position of the mounting point of the linear actuator to the ground. These coordinates were also used to calculate the distance along the rod the joint was, d. 

These coordinates were then used to calculate φ_0, the angle between the rod and the horizontal axis in the retracted state. 

The coordinates of the mass in the retracted state (c_0x, c_0y) could then be calculated using this angle, and by assuming that the mass was mounted at the absolute far end (x = 1.5) of the bounding box. Note that c_0y represents the starting height of the mass. These coordinates were also used to calculate the length of the rod, D.

Mass, m, was then caculated using a torque balance about the bottom left corner, and then solving for m given the force exerted by the linear actuator, its position along the base, its angle relative to the rod, and the length of the rod. 

Next, the position of the joint between the linear actuator and the rod after extension (b_1x, b_1y) was calculated using d, L_ext, x_0, and Heron's formula. 

Once again, these coordinates were used to calculate φ_1, the angled between the rod and the horizontal axis in the extended state.

The coordinates of the mass in the extended state (c_1x, c_1y) was calcualted using D and trig functions of φ_1.

Finally, the gravational energy function E = mg(h_1 - h_0) could be plotted using the calculated mass, g (9.8 m s^-2), and the starting/ending coordinates of the mass.

## 4) Results
Four 3D plots were generated, each one corresponding to one of the four series of linear actuators. The parameters for the linear actuator with greatest force from each series were used in the calculations. All graphs have the same X and Y axes, but the Z axis has been scaled to best show the 3D surface. 

The blue 3D surface represents the gravitational potential energy of that specific combination (recall, x axis is position of pin connecting linear actuator to the ground, y axis is the intial angle between the the ground and the linear actuator in its retracted state). The higher the blue curve is, the more optimal the combination is. 

The red 2D surfaces in the XY plane represent x_0, θ_0 combinations that are prohibited by the problem (i.e. the starting combination results in the mass starting or finishing above the top of the box, nonphysical combinations, etc...).

Therefore, optimal solutions are the highest points along the blue surface that are not above a red region. 

### 4.1) IMA22 Series
L_ret: 0.0762
L_ext: 0.3048
F_max: 1.45 kN

![Desmos Plot]({{ "assets/images/linear_actuator/IMA22/22_1.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA22/22_2.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA22/22_3.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA22/22_4.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA22/22_5.png" | relative_url }})

From inspection of the graphs, the optimal combination is:
x_0 = 1.5
θ_0 = pi/2
E = ~350

### 4.2) IMA33 Series
L_ret: 0.0762
L_ext: 0.4572
F_max: 11.34 kN

![Desmos Plot]({{ "assets/images/linear_actuator/IMA33/33_1.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA33/33_2.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA33/33_3.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA33/33_4.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA33/33_5.png" | relative_url }})

From inspection of the graphs, the optimal combination is:
x_0 = 1.5
θ_0 = pi/2
E = ~4750

### 4.3) IMA44 Series
L_ret: 0.1524
L_ext: 0.4572
F_max: 18.46 kN

![Desmos Plot]({{ "assets/images/linear_actuator/IMA44/44_1.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA44/44_2.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA44/44_3.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA44/44_4.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA44/44_5.png" | relative_url }})

From inspection of the graphs, the optimal combination is:
x_0 = 1.5
θ_0 = pi/2
E = ~6000

### 4.4) IMA55 Series
L_ret: 0.1524
L_ext: 0.4572
F_max: 35.81 kN

![Desmos Plot]({{ "assets/images/linear_actuator/IMA55/55_1.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA55/55_2.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA55/55_3.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA55/55_4.png" | relative_url }})
![Desmos Plot]({{ "assets/images/linear_actuator/IMA55/55_5.png" | relative_url }})

From inspection of the graphs, the optimal combination is:
x_0 = 1.5
θ_0 = pi/2
E = ~11500

Clearly, the IMA55 series, specifically the IMA55 RN05 is the right choice. Zooming in on the maxima, we discover the exact parameters are:

x_0 = 1.5
θ_0 = 1.616
E = ~11463.85

## Conclusion
Therefore, we can conclude that the optimal setup to maximize change in gravitational potential energy of the mass is:

Pin a rod of length 1.5077m in the bottom left corner of the box, with a mass of 3884 kg on the far right end. Pin the IMA55 RN05 linear actuator to the bottom right corner, joining it to the rod with another pin such that the linear actuator is 92.5 degrees from the ground (CCW).

