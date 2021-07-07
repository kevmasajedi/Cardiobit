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

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig1.png?raw=true" width="400px" title="Figure 1. Flow of bioelectricity in current loops, presenting four segments: an outward traversal of the cell membrane, an extracellular segment, an inward traversal of the cell membrane and an intracellular segment."> 

For modeling purposes, all the individual current dipoles can be considered to originate at a single point in space, and the total electrical activity of the heart can be represented as a single dipole, whose magnitude and direction is the vector summation of all the individual dipoles. (Madeiro, et al. 2019)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig2.png?raw=true" width="400px" title="Figure 2. The Idealized spherical torso with the centrally located cardiac source">

Let M(t) be the resultant vector which changes in the magnitude and direction as the function of time, potential distribution on the torso can be obtained by solving the Laplace’s equation as: (Gari, Francisco and Patrick 2006)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig3.png?raw=true" width="250px"> 

Where 𝜎 is the conductivity of the body electrolytic medium, |M(t)| is the magnitude of heart vector, θ(t) is the angle between the heart vector M(t) and the lead vector joining the center of the sphere to the point of surface potential measurement (Figure 2). Therefore, potential difference between two points in the torso can be given as: (Gari, Francisco and Patrick 2006)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig4.png?raw=true" width="250px"> 

Where 𝐿𝐴𝐵(𝑡) refers to the lead vector containing different points A and B of observation (e.g., electrodes) on the torso. For more elaborate discussion regarding the derivation of the abovementioned equations, please refer to (Mark 2004)

###  🎯 Design Decision I: Platform must be able to reconstruct 12 virtual leads from a reduced lead set
Equation 2.2 is important, as it states that the potential vector between two given electrodes (which essentially comprises the ECG signal in the respective lead) can be reconstructed if M(t) and the angle between M(t) and the lead vector 𝐿𝐴𝐵(𝑡) are known. As demonstrated later in the proof of concepts, the reconstruction of a virtual 12-Lead ECG from a reduced lead set is attempted in software, with satisfactory results.

However, it should be emphasized that equation 2.1 is derived with the implicit assumption of body as an idealized spherical torso, as shown in Figure 2. As a matter of fact, not only torso is far from an idealized sphere, but also it has non uniform conductance 𝜎 and its impedance can change during the cycle of respiration as well. (Gari, Francisco and Patrick 2006) So, for critical applications (e.g., in critically ill, unstable patients or in studying conditions that require exact monitoring) it is highly recommended to rely on standard 12-Lead ECG as opposed to the virtual case discussed above. 

In overall, this ability is helpful in that it provides the clinicians in unprivileged areas with more data without additional costs. Thus, it helps in cases which wide availability of ECG monitoring is the primary goal. For example, when secondary prevention (i.e., screening) of ischemic or heart rhythm disease is to be performed on a disperse population in a large geographic area.

***

## Signal Acquisition:
### Electrode-Patient Interface:
The electrical activity of the heart is propagated throughout the body by ions. To measure this activity at body surface, conversion of ions to electrons, is needed. This conversion happens at electrode-patient interface. When a metallic electrode (most commonly silver-silver chloride) is in contact with an electrolyte (skin or electrode gel), an electrochemical reaction occurs by ionexchange. Metal atoms (𝑀) tend to lose n electrons and pass into the electrolyte as metal ions (𝑀+𝑛 ) causing the electrode to become negatively charged with respect to the electrolyte: (Webster 2006)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig5.png?raw=true" width="170px"> 

Similarly, under equilibrium, ions in the electrolyte take the reverse direction of the equation (3.1). The electrode becomes positively charged with respect to the electrolyte as a result. (Webster 2006)


<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig6.png?raw=true" width="170px"> 

Under equilibrium condition, rate at which metal atoms lose electrons and pass into the electrolyte is exactly balanced by the rate of equation (3.2). Thus, the current flowing in one direction, cancels out the current flowing in the opposing direction, resulting in zero net current flow. However, a potential difference is found to exist between the electrode and electrolyte and depends on the position of the equilibrium between two processes (3.1) and (3.2). This potential is termed as half-cell potential, and can be calculated by the Nernst Equation (3.3) (Webster 2006)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig7.png?raw=true" width="450px" title="𝐸0 is the standard half-cell potential measured relative to the standard hydrogen electrode, R is the universal gas constant, n is the number of electrons involved in the reaction, T is the absolute temperature in Kelvin. F is the faraday’s constant.">

Since an ECG signal is obtained by two equal electrodes for each lead, it can be assumed that half-cell potentials are equal and offset voltage at the input of differential amplifier is zero. In practice, small differences in the properties of electrodes result in a dc offset voltage, which can sometimes change over time. (Madeiro, et al. 2019)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig9.png?raw=true" width="250px" title="Figure 3. Electrode-Patient Interface Equivalent Circuit. 𝐶𝑑𝑙 the double-layer capacitance; 𝑅𝐶𝑇 the charge transfer resistance; 𝑅𝑇𝑂𝑇𝐴𝐿 lead and electrode resistances and 𝐸𝑅𝐸𝑉 the equilibrium (Nernst) potential.">

Further, as positive ions are attracted by the electrode and negative ions by the electrolyte, a double layer capacitance is created at the electrode-patient interface. Also, due to the electrochemical reactions in the interface, some dc current can leak across the double layer. The amount of leak is proportional to charge transfer resistance which is calculated by the equation (3.4) where 𝑖0 is the current flowing across the interface in both directions (no flow under equilibrium, as discussed before). The equivalent circuit is shown in Figure 3. (Madeiro, et al. 2019) Note that equations (3.3) and (3.4) are only of theoretical interest here.

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig8.png?raw=true" width="170px"> 

###  🎯 Design Decision II: Cost-optimized solutions should be supplied with higher voltage
DC offset voltage discussed above, can be as high as 300mV which is enormous when compared to typical 1.8mV amplitude of the ECG signal (Lee and Kruse 2008). The dc offset can be reduced by the use of high-quality electrodes or significantly suppressed with specialty circuits (Huang, Huang and Li 2018). However, in accordance with the design principles discussed in the abstract of this article, a simple rule of thumb is proposed instead: Use higher supply voltage, when designing cost-optimized solutions.

To put this design decision in better context, we need to slightly deviate from the flow of the discussion:

>As ECG signal is very weak with only 0.5µV – 4mV of amplitude and is susceptible to interference [e.g., 50 or 60Hz AC noise which is present in virtually all indoor environments], it needs to be amplified before processing and analysis. (Huang, Huang and Li 2018)

>A high dc offset voltage can quickly saturate a low-voltage circuit and limit the amount of achievable amplification. For example, a 3V supply voltage (equivalent to -1.5v to +1.5v dual supply), can be easily saturated by only a 5 times front-end amplification. Thus, only 2x to 3x first-stage gain can be reasonably applied. A high-resolution signal can still be obtained after a second-stage amplification and by the use of a high-precision analog-todigital converter (ADC). But as discussed later, due to low front-end amplification the noise profile will not be satisfactory. In addition, high-precision ADCs can be costly and usually have far slower sampling rates.

Thus, for designing cost-optimized solution, a supply voltage of at least 12V (-6.0v to +6.0v) is recommended. As CardioBit is designed for battery-powered operation, and high-capacity 3.7v 18650 Lithium-Ion cells are widely available with ever decreasing cost (Warner 2015), such supply voltage can be provided with 3 to 4 cells in series, allowing for a compact form-factor for the final, assembled device.

***

### Surface Lead Systems:
The standard 12-Lead ECG is a noninvasive representation of the electrical activity of the heart. This lead system rests on the theoretical foundation of classical lead theory introduced by Einthoven et al. (Madeiro, et al. 2019) This theory assumes a single time-varying dipole in 2- dimensional space to represent electrical activity of the heart, and assumes human body as a homogenous conductor. Later, Burger and van Millan improved and generalized the theory (Burger and van Milaan 1948) with regard to 3-dimentionality and non-homogenous conductance of the body. Yet, their theory still rests on the fixed-dipole hypothesis, thus allows for the derivation of the potentials at a given instance, anywhere on the body surface (Macfarlane, et al. 2011). As noted in [Design Decision I](##Design-Decision-I), these assumptions are not completely solid.
