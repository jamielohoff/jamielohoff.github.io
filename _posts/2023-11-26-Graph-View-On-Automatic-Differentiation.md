---
title: "Graph View on Automatic Differentiation"
tags:
  - AD
use_math: true
comments: true
---

Few people are familiar with the graph view on automatic differentiation, although it
is quite helpful for understanding what automatic differentiation (AD) actually does.
The underlying idea is to frame AD as an operation on the computational graph.
This short blog article tries to give an overview over this concept called 
**vertex elimination** and tries to describe in detail how it works.
Consider a simple function $f$ that maps two inputs to two outputs:
$$
f(x_1, x_2) = \begin{pmatrix} \log \sin x_1x_2 \\ x_1x_2 + \sin x_1x_2 \end{pmatrix}
$$ 
<br>
The first thing we need is the computational graph. We obtain this graph by first 
dissecting the function $f$ into a sequence of elemental operations like 
additions, multiplications, logarithms etc. This step is always part of every
AD algorithm/method because for these simple functions we know the exact (partial) 
derivatives. With the chain rule, we can then assemble the Jacobian of $f$ from 
these simple elemental partial derivatives. Most AD algorithms then differ 
in the order in which the chain rule is applied, e.g. forward-mode vs. backpropagation.
After we have dissecting $f$ into its elemental operations we store them in
sequential order in a list called the **Wengert List** or the **Tape** (see figure 1).
Every elemental operation outputs an intermediate variable $v_i$ which is then
fed to the next operation.

fig here!

Not that the naming convention is a bit weird as we index the using negative 
numbers up to 0, while the first elemental operation is indexed using 1.
The reason for this will become clear in just a moment.
To construct the computational graph, we now draw a bunch of vertices, one for 
each input variable, operation and output variable. The edges that connect the
vertices just represent the data dependencies of the inputs, operations and outputs.
For example, since the addition depends on both input variables, $x_1=v_{-1}$ and
$x_2 = v_0$, the vertex $v_1 = v_{-1}v_0$ has two edges connecting them. Since
the vertex $v_1$ is involved in computing $v_2$ and $v_4$ it has to edges emanating 
to these vertices, making a total of two ingoing and two outgoing vertices.
Note that input vertices are red and only represent numbers, while intermediate and
output vertices are blue and green respectively. The latter two only differ in the
fact that an output vertex has no outgoing edges. Figure 2 provides a nice visual
account of the computational graph creation.

{% include figure popup=true image_path="/assets/cce/GraphConstruction.gif" alt="Computational Graph Creation" caption="Figure 2: This animation shows how the computational graph is created from the tape that stores the elemental operations that make up function f. Input variables are red, intermediate elemental operations and corresponding variables are blue and output vertices are green." %}

The first step towards the vertex elimination algorithm is to populate the edges
of the graph with partial derivatives of the destination vertex with respect to
the starting vertex. For example for the edge going from vertex $v_{-1}$ to vertex 
$v_1$, we would compute $\dfrac{\partial v_1}{\partial v_{-1}} = \dfrac{\partial}{\partial v_{-1}} v_{-1}v_0 = v_0$ 
and add this to the edge as an associated value.
We introduce the shorthand $c_{ji}$ = $\dfrac{\partial v_j}{\partial v_i}$ for
the partial derivative value associated with the edge going from $i$ to $j$.
Note that we add the computed partial derivatives to the tape as well.
In general, we will utilize the tape as a record keeping devices that stores
all the operations that have been computed "on the graph".
In essence, it is an abstract representation of a computer program that we could
potentially write in a language of our choice, e.g. C++, Python or Julia.

{% include figure popup=true image_path="/assets/cce/PopulatingDerivatives.gif" alt="this is a placeholder image" caption="Figure 3: The animation demonstrates the population of the graph's edges with partial derivatives. Note that
for every computed derivative we add an entry to the tape." %}

Now it is time to describe the actual vertex elimination algorithm.
For this, we take a look at the second intermediate (blue) vertex.
It has one ingoing and two outgoing vertices. 
The vertex elimination rule for vertex $j$ now states:
<br>
*For every ingoing edge of vertex $j$ we multiply the corresponding partial 
derivate $c_{ji}$ with every partial derivative of every outoing edge $c_{kj}$.
For every of these products, we draw a new edge from the starting vertex $i$ of
the ingoing edge to the destination vertex $k$ of the outgoing edge and identify
the associated partial $c_{ki} = c_{kj}c_{ji}$. If said edge already exist, we just
add the values, i.e. $c_{ki} = c_{ki} + c_{kj}c_{ji}$. Finally we delete all edges
connected to vertex $j$.*
<br>

Figure 4 gives an animated example of how this works for our function $f$.
Note that this is nothing else than locally applying the chain rule to vertices
$i$, $j$ and $k$. As a quick illustration, look at the very simple graph $x \to g(x) \to f(g(x))$.


{% include figure popup=true image_path="/assets/cce/Vertex2Elimination.gif" alt="this is a placeholder image" caption="Figure 4:." %}

{% include figure popup=true image_path="/assets/cce/Vertex1Elimination.gif" alt="this is a placeholder image" caption="This is a figure caption." %}


