# Motivation
The ability to measure time with high precision and accuracy is an essential requirement for not just scientific exploration but also for communication, navigation and a plethora of technological applications. 
Clock synchronization determines the time difference between spatially separated clocks. 
With the increasing popularity of quantum information, quantum entanglement promises to be a useful platform for several applications of communication. This is the spirit that we apply to our project, quantum clock synchronization.

<p align="center">
  <img src="https://github.com/user-attachments/assets/4e65181d-a7fb-40cb-b565-21c4a80e8e18" width = 300/>
</p>

Classically, clock synchronization algorithms either make use of a client-server model or they employ a decentralized approach where nodes coordinate directly with each other to align their clocks. In the client-server model, the client sends a request to a server (which has a more accurate clock), and the server responds with a time stamp. The client then estimates the round-trip network delay and adjusts its own clock accordingly. Algorithms like **Cristian’s Algorithm** and **Network Time Protocol (NTP)** work this way, where the client uses the server's timestamp along with the round-trip delay to adjust its local clock. In contrast, decentralized models like the **Berkeley Algorithm** allow nodes to synchronize their clocks with each other without relying on a single authoritative time server. In this model, one node acts as a coordinator, periodically requesting the time from other nodes, calculating the average time, and sending adjustment messages back to each node.

Classical clock synchronization protocols, while effective, face several challenges that can be addressed by quantum clock synchronization. One major issue is network delay, as classical protocols like NTP and Cristian’s algorithm rely on estimating round-trip time, which can be asymmetric and unpredictable. Quantum synchronization, using entangled particles, can avoid this by providing instantaneous and precise time transfer, regardless of network conditions. Furthermore, while classical systems struggle with maintaining accuracy over long distances, quantum entanglement allows for synchronization over vast distances without degradation, offering better scalability and precision, especially in large networks. 

# Working Principle
First consider a qubit with stationary states in the computational basis as $\ket{0}$ and $\ket{1}$. 
Starting with the initial state, a maximally entangled bell state:

$$\ket{\psi} = \frac{\ket{00}+ \ket{11}}{\sqrt{2}} = \frac{\ket{+}_A \ket{+}_B + \ket{-}_A \ket{-}_B}{\sqrt{2}}$$  

Here A and B refer to the qubit distributed to Alice and Bob. Alice and Bob both measure their respective qubits at $t=0$. Their clocks being unsynchronized leads to Bob measuring the qubit later than Alice, with a gap of $\Delta$.


When Alice measures $\ket{+}$, Bob's qubit immediately collapses to $\ket{+}$. In the $\Delta$ between their measurements, Bob's state evolves with time according to Schrödinger's Equation : 

$$\ket{\psi_B} = \frac {\ket{0} + e^{-i \omega \Delta}\ket{1}}{\sqrt{2}}$$  

Where, $$\omega = \frac{E_1 - E_0}{\hbar}$$

$E_1$ and $E_0$ are the energy levels of the $\ket{1}$ and $\ket{0}$ states.  


This program uses rotation operators to simulate time evolution.


Bob obtains $\ket{1}$ with probability

$$P(\ket{+}) = \langle + | \psi_B \rangle = \frac{1 + cos(\omega \Delta)}{2}$$  

This probability allows Bob to sample and calculate for $\Delta$

# Algorithm Summary
This algorithm for multiparty clock synchronization is infitely scalable. This program implements this for two and three node networks.
* The initial state is a quantum W-state :

  $$\ket{\psi} = \frac{1}{\sqrt{n}}(\ket{10...00} + \ket{01...00} + \ket{00...01})$$
* Alice, at standard time $t_A = 0$ measures the qubit in her possession in the $(\ket{+},\ket{-})$ basis .
  - Alice is taken as the standard without loss of generality, and synchronization is performed relative to her clock.
* Alice broadcasts her measurement result across the network.
* Every node performs measurement based on their respective clocks at $t=0$, which has a difference of $\Delta$ from the standard clock.
* If Alice measured $\ket{+}$, the others obtain $\ket{+}$ upon measurement with probability:

$$P(\ket{+}) = \frac{1}{2} + \frac{cos(\omega \Delta)}{n}, \omega = \omega_2 ,\omega_3 ... , \omega_{n}$$
* Over many iterations, the others can deduce the value of $\Delta$.
  - For $|\omega \Delta|<2 \pi$, the others can adjust their clock accordingly.

# Usage
The 2 programs implement the synchronization protocol for a network with 2 nodes and 3 nodes.

The inputs for both programs are in the parameters files. The input variables are $\omega$ and $\Delta$, the time delay used in the simulation.

The number of iterations can also be varied. Since the program works on the principle of sampling probability distributions, increasing the number of iterations gives more accurate results.

The output is the calculated time delay between the nodes.

# References
1. [Shi, J., Shen, S. A clock synchronization method based on quantum entanglement. _Sci Rep_ 12, 10185 (2022)](https://www.nature.com/articles/s41598-022-14087-z)  
2. [X Kong, T Xin, SJ Wei et al. Implementation of Multiparty quantum clock synchronization. arXiv:1708.06050](https://arxiv.org/abs/1708.06050)   
3. [Kómár, P., Kessler, E., Bishof, M. _et al._ A quantum network of clocks. _Nature Phys_ 10, 582–587 (2014)](https://www.nature.com/articles/nphys3000)   
4. [M Krčo, P Marko. Quantum clock synchronization: Multiparty protocol. Phys. Rev. A 66, 024305 (2022)](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.66.024305)   

# Credits
This repository was developed by [Hersh Samdani](https://github.com/hershsam) and [Yash Kashtikar](https://github.com/YashKash63), students of Engineering Physics at Indian Institute of Technology, Madras.
