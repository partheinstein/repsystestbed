# Introduction #
There are many trust models in the literature that address the problem of computing trust in agent systems but differ in the type of rating used (satisfaction ratings, certifications and trust ratings), in whether they calculate global or local trust and, finally, in how the rating is used to decide whether to trust an agent. This makes it challenging to compare and evaluate them against attack scenarios such as self-promoting, white-washing,
slandering, and introducing Sybils. To analyze the performance of trust models, we need a testbed that is generic enough to accommodate as many reputation systems as possible. Repsyst aims to fill this requirement.

# Details #
The model for a testbed is essentially based on the following concepts:
-  Trust is a process that occurs in stages and existing trust models t in various stages of the trust process.
- Both trust models and their evaluations are viewed as workflows using Petri net

The input and output to trust algorithms are modeled as graphs which can be of type Feedback History Graph (FHG), Reputation Graph (RG) or a Trust Graph (TG). Nodes in these graphs represent agents in a system. A FHG is one where the edges are labeled as feedbacks by the source node on the sink node. A RG is a graph where edge weights represent the trustworthiness of the sink of the edge from the perspective of the source node and a TG is a graph where a presence of edge indicates trust.  Thus, the trust process can be viewed as a workflow and workflows can be modeled using Petri Nets.

**Input and Output of Algorithms**
We represent an algorithm as transitions, input places as input parameters, and output places as output parameters of the algorithm. The places can be graphs, boolean values, numerical values, agents, etc. The signature of the algorithm is defined by the structure of a Petri net. The table below provides examples of algorithm signatures and their corresponding Petri net structures.

![https://repsystestbed.googlecode.com/files/TableAlgSamplesAndPetriNets.png](https://repsystestbed.googlecode.com/files/TableAlgSamplesAndPetriNets.png)

**Token as Events**
A token in a place indicates that an event associated with that place has occured and therefore the place is ready to processed. Our testbed allows the experimenter to specify how a token is associated with a change in a place. For example, a token may be put in an FHG place after n number of feedback is added to the graph. In another example, a token may be placed in an RG place to indicate that the graph has been updated.