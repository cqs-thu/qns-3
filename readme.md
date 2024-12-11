# Qns-3: A Quantum Network Simulator Based on ns-3

Qns-3 is a quantum network simulator built on top of the widely used [ns-3](https://www.nsnam.org) network simulation platform. It provides APIs consistent with ns-3 conventions, ensuring ease of integration for users familiar with ns-3. The aim of qns-3 is to provide researchers and developers with a powerful simulation tool to explore, benchmark and innovate in the rapidly evolving field of quantum networks.

Developed by members of the Research Center for Quantum Software at the Department of Computer Science and Technology, Tsinghua University, qns-3 is an ongoing project that will evolve alongside advancements in quantum network simulation techniques. The current authors of this project are:
- Huiping Lin
- Ruixuan Deng
- Chris Z. Yao
- Zhengfeng Ji
- Mingsheng Ying

Qns-3 leverages discrete-event simulation to model quantum networks and emphasize the need to take noise into account in simulations. It incorporates tensor network techniques to efficiently represent and manipulate quantum information objects including quantum states, operations and noises in quantum network simulation. It uses [ExaTN](https://github.com/ORNL-QCI/exatn) (a software library for expressing, manipulating and processing arbitrary tensor network) as the basic engine. The present version of qns-3 emphasizes the control flow adaptation method, a foundational technique designed to enhance the efficiency of tensor-based simulation backends.

## Discussion of Key Features

### Network Model

The quantum network model incorporated in the qns-3 simulator involves two general classes of components: quantum nodes and quantum channels. Quantum nodes are entities that manage all equipments capable of processing qubits locally. Quantum channels, such as optical fibers, refer to all communication channels that can transmit quantum information.

#### Quantum Node

Quantum nodes are similar to the end hosts in classical networks, which are connected to other quantum nodes by quantum channels.
Each quantum node is equipped with network hardwares and software stacks. For the classical network stack, we reuse the ns-3 internet stacks and can support any classical network topology. The model of quantum network protocol stack is still under exploration today. In qns-3's current implementation and futural development, we follow a quantum analogue of the classical OSI 5-layer model.

#### Quantum Channel

A quantum channel connects two quantum nodes and supports the transmission of qubits between the two nodes.
The physical material of quantum channels may be polarization-maintaining optical fibers.
Quantum channels also have the ability to transmit classical information by default.
Quantum channels exhibit inherent noises, potentially leading to qubit errors due to distinct physical attributes associated with each channel.
Hence, specific noise parameters are attributed to each quantum channel.

### Control Flow Adaption (CFA) Method

#### Research Motivation

In quantum networks, many protocols can be modeled in the LOCC (local operations and classical communication) scheme.
In these protocols, classical information received by different parties influences future quantum operations, leading to the required construction of a complex backend to track quantum states if done straightforwardly.
For example, the tensor network method is a powerful technique for classical simulation of quantum information, but as the protocols become more intricate and the number of qubits increases, the contraction complexity of the tensor networks often grows exponentially.
The need to simulate the noise behavior in quantum networks marks as a further challenge for tensor network methods as noises add many new contractions in the tensor network representation.
Additionally, quantum measurements, which bring randomness in the simulation of quantum network protocols, are another source of complications.
To obtain an average simulation result when verifying a protocol, one may need to employ a Monte Carlo method and run the simulation a large number of times.

#### Key Technique

The CFA method performs a control flow analysis to determine the necessary information required for future rounds of the LOCC protocols.
By compressing and adapting the classical control flow in a way that aligns with the capabilities of tensor networks, we can efficiently handle the complexities of many quantum network protocols.
This careful adaptation ensures that the control flow information integrates seamlessly into the tensor network simulation framework.
Instead of faithfully simulating all quantum measurements and reveals one possible branch, this method provides a mechanism that is able to integrate all possible results in a single round. 

For a more detailed theory of our CFA method, please refer to our paper to be published in INFOCOM 2025.
