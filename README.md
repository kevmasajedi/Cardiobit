# CardioBit
## A Smart, Opensource and Affordable Electrocardiography Platform
CardioBit is an architecture for building IoT capable, precise, scalable and cost-efficient ECG Devices. IoT-capability and affordability are the central dogmas of this architecture. This is mainly achieved by using opensource hardware, minimum reliance on specialty components, offloading signal processing tasks to the server and avoiding the use of costly isolation components - yet ensuing safety by relying solely on battery power.

## Introduction
In the wake of COVID-19 pandemic, in many countries throughout the world, medical infrastructure is seriously overwhelmed by a novel coronavirus. The vast majority of medical equipment are heavily patented and are sold in small volumes. Further, there is evidence that some entities actively prevented medical treatments from being deployed, even in the current pandemic. This model obviously fails during critical times. On the other hand, deployment of opensource hardware and software platforms, can result in significant cost reduction as well as increased critical preparedness. (Pearce 2020)

The potential of opensource platforms to facilitate the formation of inclusive, potentially worldwide research communities, have also captured my imagination. Economically
viable solutions can be derived out of the core architecture discussed here, and can be manufactured and deployed throughout the world, in a distributed fashion. In additon, a 12-lead proof-of-concept hardware is provided.


## Background
Readers are assumed to have a minimal background in physiology and electrical engineering. Namely, they are assumed to be familiar with the basic morphological characteristics of electrocardiogram and their physiological significance, and familiarity with ohm’s law, impedance, basic circuit theory, RC analog filters and basic operational amplifier circuits. However, prospective readers are encouraged to continue, as proper reference for further consultation is given in due course. For introductory topics in cardiac physiology see (Guyton and Hall 2015), and for basic introduction to electrical engineering concepts regard (Scherz and Monk 2016) and (Mancini and Carter 2009)

The simplest model for relating the electrical activity of the heart to the body surface potentials, is the single dipole model. As an action potential propagates through the cell, in the interface of depolarizing and resting tissue, there’s an intracellular current flow in the same direction of the action potential propagation. There is also an equal extra cellular current flowing against the direction of propagation. Therefore, charge is conserved and all the current loops in the conductive media form a single field, called “dipole field” (Figure 1). (Gari, Francisco and Patrick 2006)

For modeling purposes, all the individual current dipoles can be considered to originate at a single point in space, and the total electrical activity of the heart can be represented as a single dipole, whose magnitude and direction is the vector summation of all the individual dipoles. (Madeiro, et al. 2019)

Let M(t) be the resultant vector which changes in the magnitude and direction as the function of time, potential distribution on the torso can be obtained by solving the Laplace’s equation as: (Gari, Francisco and Patrick 2006)

Where 𝜎 is the conductivity of the body electrolytic medium, |M(t)| is the magnitude of heart vector, θ(t) is the angle between the heart vector M(t) and the lead vector joining the center of the sphere to the point of surface potential measurement (Figure 2). Therefore, potential difference between two points in the torso can be given as: (Gari, Francisco and Patrick 2006)

Where 𝐿𝐴𝐵(𝑡) refers to the lead vector containing different points A and B of observation (e.g., electrodes) on the torso. For more elaborate discussion regarding the derivation of the abovementioned equations, please refer to (Mark 2004)
