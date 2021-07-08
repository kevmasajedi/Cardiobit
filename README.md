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
The standard 12-Lead ECG is a noninvasive representation of the electrical activity of the heart. This lead system rests on the theoretical foundation of classical lead theory introduced by Einthoven et al. (Madeiro, et al. 2019) This theory assumes a single time-varying dipole in 2- dimensional space to represent electrical activity of the heart, and assumes human body as a homogenous conductor. Later, Burger and van Millan improved and generalized the theory (Burger and van Milaan 1948) with regard to 3-dimentionality and non-homogenous conductance of the body. Yet, their theory still rests on the fixed-dipole hypothesis, thus allows for the derivation of the potentials at a given instance, anywhere on the body surface (Macfarlane, et al. 2011). As noted in [Design Decision I](https://github.com/kevmasajedi/Cardiobit/blob/main/README.md#-design-decision-i-platform-must-be-able-to-reconstruct-12-virtual-leads-from-a-reduced-lead-set), these assumptions are not completely solid.

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig10.png?raw=true" width="300px" title="Figure 4. Bipolar Limb Leads.">

12 Leads on this system are composed of 3 bipolar limb leads(I, II, III), 3 augmented unipolar limb leads (aVR, aVL and aVF) and 6 unipolar chest leads (𝑉1 𝑡𝑜 𝑉6). Bipolar limb leads are defined by equations 3.5, 3.6, 3.7. (Figure 4). Right leg electrode is used for reducing electromagnetic interference, and can be placed anywhere at body surface. It is placed at right leg for convenience. (Madeiro, et al. 2019).

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig11.png?raw=true" width="200px" >

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig12.png?raw=true" width="300px" title="Figure 5. Position of unipolar chest leads">

Potentials of unipolar chest electrodes (𝑉1 𝑡𝑜 𝑉6) is measured relative to Wilson Central Terminal (WCT), which is the normalized sum of the potentials of 𝐸𝐿𝐴, 𝐸𝑅𝐴 and 𝐸𝐿𝐿. (Equations 3.8 and 3.9) (Madeiro, et al. 2019)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig13.png?raw=true" width="250px" >

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig14.png?raw=true" width="300px" title="Figure 6. Standard 12-Lead System">

Augmented unipolar limb leads, comprise the remaining three leads. The same set of electrodes from bipolar leads are used (𝐸𝐿𝐴, 𝐸𝑅𝐴 and 𝐸𝐿𝐿) as positive electrodes and negative electrode is a modified version of the Wilson Central Terminal called of GT. (equations 3.10 through 3.12) (Madeiro, et al. 2019)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig15.png?raw=true" width="350px" >

###  🎯 Design Decision III: Technical issues should be openly discussed, anyone’s contribution is to be welcomed
A variety of other lead systems are proposed, including Frank, reduced Frank, EASI or various three or two lead configurations. (Gari, Francisco and Patrick 2006). But the standard 12-Lead system has by far the widest adoption in clinical practice. Thus, other configurations are not discussed further. 

However, CardioBit is opensource in hardware and software and it can be adopted for any given lead set, by any interested researcher. In fact, as the author, I’ll be delighted If I see such actions taken by the potential research and development community. I believe community, can shed more light on the issue of practicality of virtual 12-Lead ECG reconstruction from a reduced lead set, provide a quantitative measure of accuracy for such reconstructions, or even come up with a more theoretically solid method of reconstruction (e.g., based on the modern concept of leadfield instead of lead-vector)

In that spirit, I like to echo a section of *ST Segment Monitoring Practice Guideline Working Group*’s recommendation regarding the use of reduced lead sets, which is further discussed by (Gari, Francisco and Patrick 2006):

- If only two leads are available for ST segment monitoring (for patients with acute coronary syndrome), lead III and V3 should be used.
- The best three-lead combination is III-V3-V5
- V1, is considered the best lead to monitor for cardiac arrhythmias
- The use of at least three chest leads (V3, V4, V5) is recommended for ST analysis

The provided proof-of-concept of the CardioBit platform’s core architecture, is designed for acquisition of 2 bipolar limb leads and 2 unipolar chest leads. User (whether clinician, researcher or developer) can decide which respective two leads he or she wants to use. As noted, reconstruction of virtual 12-lead ECG from the chosen leads is attempted in software. It is left up to user to decide whether to use this feature or not. For more information, please regard the factors noted in  [Design Decision I](https://github.com/kevmasajedi/Cardiobit/blob/main/README.md#-design-decision-i-platform-must-be-able-to-reconstruct-12-virtual-leads-from-a-reduced-lead-set)
***
## Electrical Design
### The Characteristics of Measured Biopotential

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig16.png?raw=true" width="400px" title="Figure 7. Components of measured biopotential. (Credit: Madeiro, et al. 2019)">

As stated before, surface potentials representing ECG signal are very weak (a few millivolts maximum) and are riding on a comparatively enormous offset voltage (hundreds of millivolts). (Lee and Kruse 2008) To complicate matters further, these signals are often contaminated with biopotential activity of muscles and nerves beneath the electrodes, 50Hz/60Hz AC powerline interference, low frequency motion artifacts and all sorts of high frequency electromagnetic noise (Madeiro, et al. 2019)

Among these parasitic sources, the AC interference may be the most troublesome. Low frequency motion artifacts can be dealt with by good skin preparation and good electrode adhesion. Also, biopotential artifacts of nerve and muscle activity will be negligible if patient remain still and relaxed at a comfortable position. Lastly, high frequency noise, can be easily suppressed by a simple analog RC filter and later by digital filtering. Conversely, AC interference is present virtually anywhere and its frequency falls in the normal frequency range of ECG signal (0.05Hz – 250Hz)

Unfortunately, AC interference is unavoidably present in any clinical situation. This interference can be of two independent types: magnetic and electric: A changing magnetic field produced by AC powerlines can induce in any nearby conductive loop, an electromotive force, which results in an ac potential. A changing electric field can also produce interference by causing ac currents to flow to ground through the system. (Huhta and Webster 1973)

The magnetic interference is directly proportional to the area of the loop formed between two electrode leads and reducing this area is the best way to reduce this type of interference. Expensive shielding of the wires does not prevent this kind of interference. (As shown in Figure 8.) (Huhta and Webster 1973)

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig17.png?raw=true" width="400px" title="Figure 8. AC noise pickup by magnetic interference. (Credit: Huhta and Webster 1973)">

The electrical interference is modeled as the capacitive coupling of AC powerlines to lead wirings, which results in displacement current induced in the respective leads, because the length of the leads is almost the same and the wires run closely together, this displacement current should be approximately equal in each lead. (Huhta and Webster 1973)

Assuming that electrodes have perfectly the same electrical impedance, such displacement current should be rejected. Because, the front-end differential amplifier, only amplifies the difference between the voltages of the two inputs. In other words, as impedance of both electrodes is the same, the displacement current, results in induction of exactly the same voltage in them. This can be easily stated using generalized ohm’s law (Equation 3.13).

<img src="https://raw.githubusercontent.com/kevmasajedi/Cardiobit/main/readme_images/fig18.png?raw=true" width="400px" >

Where 𝑉_𝐴𝐶 is the AC interference potential, 𝐼_𝐷 is the displacement current and Z is the impedance in respective inputs A and B. It is observed that 𝑉_𝐴𝐶 is induced by unbalance of the two impedances (the term 𝑍_𝐴 − 𝑍_𝐵 being non-zero). (Huhta and Webster 1973)

That being said, it should not come as a surprise, that no two electrodes are perfectly the same in real world. Electrode impedance at 60Hz may range from less than 1 KΩ up to 100 KΩ. Given typical values of 6 nA of 𝐼𝐷 in a 3-m length of unshielded wire, and typical impedance imbalance of 5KΩ, induced potential (𝑉𝐴𝐶) would be about 30µV which is roughly 3 percent of the ECG potential. (Huhta and Webster 1973)

Author should note here that in the experimentation with lower quality, cheaper disposable electrodes, far higher impedance imbalance has been observed and induced potential frequency observed to be as much as 10% of the QRS potential. Such observations influenced the **Design Decision IV** which will be discussed in due course.


