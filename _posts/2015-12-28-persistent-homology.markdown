---
layout: post
title:  "Persistent Homology"
---

This is a set of learning notes on persistent homology.

Persistent homology is a method in topological data analysis. It views the date set(refered as a data point cloud) as a geometric object, and extracts topological information (homology groups) of the set. We first review the main idea of homology and persistence, and then briefly discuss a few applications of persistent homology.

### Homology

Topology studies the properties of spaces that remain unchanged under continuous deformations. An example of topological properties is the number of connected components of a space. Given a space \\( X \\), a connected component of \\( X \\) is a subset of \\( X \\) that is not the union of two disjoint ope subsets (in some sense, this means the the subset is one piece instead of consisting of several pieces), and frequently, this is equivalent to saying that any two points of this subset can be joined by a continuous path in this subset. It is intuitively clear that continuous deformation cannot change the number of connected components: to create a new connected component, one needs to "tear" the space apart, and to eliminate a connected component, one needs to "stitch" two disconnected parts, while those two operations are not continuous.

Now consider the following example: the plane \\( \mathbb{R}^2 \\) and the punctured plane \\( \mathbb{R}^2  \backslash \{ 0 \} \\), we have the following observation:

In \\( \mathbb{R}^2 \\), any loop goes through \\( (1, 0) \\) can be continuously deformed into a single point \\( (1, 0) \\); while in \\( \mathbb{R}^2  \backslash \{ 0 \} \\), we see a loop \\( \gamma \\) that wraps around $ (0, 0) $ can not be continuously deformed into a single point. If we regard all the loops (that goes through a fixed point $ (1, 0) $) that can be continuously deformed into each other as a single object, and use $ [ \gamma ] $ to denote such an object, we see $ [ \gamma_0 ] \neq [\gamma_1 ] $, and actually $ [ \gamma_2 ] $ is yet another different object. Those different "loops" form the elements of so called fundamental group $ \pi_1 (\mathbb{R}^2 \backslash \{ 0 \}, (1, 0) ) $. In particular, we see the fundamental groups can be used to tell the topological difference of spaces.

However, higher dimensional homotopy groups $ \pi_n(X) $ are very hard to compute. For example, for a simple space such as the two dimensional sphere $ S^2 $, we have:
$$
\begin{array}{l | c c c c c c c c c c }
\hline
n & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10\\ \hline
$ \pi_n(S^2) $ & 0 & $ \mathbb{Z} $ & $ \mathbb{Z} $ & $ \mathbb{Z}_2 $ & $ \mathbb{Z}_2 $ & $ \mathbb{Z}_{12} $ & $ \mathbb{Z}_2 $ & $ \mathbb{Z}_2 $ & $ \mathbb{Z}_3 $ & $ \mathbb{Z}_{15} $ \\ \hline
\end{array}
$$

And the computations are nontrivial.

A more computable topological object is the homology group $ H_n(X) $. Similar to homotopy groups, homology groups have elements that are essentially different ways to "putting" simplices of various dimensions into a space $ X $. This is the idea of the singular homology, but we will consider a simpler class of spaces that are themselves made of simplices (this class of spaces is usually adequate for most applications). Consider the following space: figure of the simplicial complex.

This space consists of $ 7 $ vertices, $ 6 $ edges and $ 1 $ triangle. Note all these elements are simplices of varous dimensions: a vertex is a simplex of dimension $ 0 $, an edge is a simplex of dimension $ 1 $ and a triangle is a simplex of dimension $ 2 $. These simplices are also nested nicely: every intersection of two $ 1 $-simplex is a $ 0 $ -simplex that is in the description of $ X $, and every lower dimensional face of a simplex is a simplex that is also in the description of the space. These are essentially the defining properties of a nice space that consists of simplices -- the simplicial complexes.

Though it is visually clear that $ X $ has $ 3 $ connected components, we will define the compute this number using the language of homology.

First, there are $ 7 $ vertices, and $ (b, c) $ form the boundary of a $ 1 $ simplex $ e_{b c} $ (the edge $ b c $), and so do $ (d, e), (e, f), (f, g), (g, d) $ and $ (d, f) $, while $ a $ isn't part of boundary for any $ 1 $-simplex. Also note, since there is no  $ (-1) $-dimensional simplex, all the vertices themselves are "boundaryless". So get the following:

Space $ X $ has $ 7 $ boundaryless $ 0 $-dimensional simplices, and some pairs of them form boudary of $ 1 $-dimensional simplices.

Now if we regard those vertices that form a boundary of an $ 1 $-simplex as equivalent (in other words, as a single object), then we get the following list of equivalent objects:

$$
\begin{eqnarray*}
a, \\
b \sim c, \\
d \sim e \sim f \sim g,  
\end{eqnarray*}
$$

i.e., there are $ 3 $ equivalent classes of boundaryless $ 0 $-simplices. Note the number $ 3 $ is exactly the number of connected components.

Since those $ 3 $ classes are "independent" of each other, we can regard each of them as a basis element. Especially, if we consider homology with coefficients in a field (for example the real numbers $ \mathbb{R} $), then $ 3 $ basis elements generate a $ 3 $-dimensional vector space. We denote $ H_0(X) = \mathbb{R}^3 $. $ H_0(X) $ is called the $ 0 $-dimension homology group of $ X $ (with coefficient in $ \mathbb{R} $).

To generalize this to other dimensions, first recap how $ H_0(X) $ is defined. Let $ Z_0(X) $ be the vector space generated by boundaryless $ 0 $-dimensional simplices, in this case, $ Z_0(X) = \mathbb{R}^7 = \mathrm{Span} \{ a, b, c, d, e, f, g \} $. Let $ B_0(X) $ be the vector space generated by boundaries of $ 1 $-simplices, in this case, $ B_0(X) = \mathbb{R}^4 = \mathrm{Span} \{ c - b , e - d, f - e, g - f, d - g \} =  \mathrm{Span} \{ c - b , e - d, f - e, g - f \} $. Here in the expression $ c - b $, the negative sign " - " indicate the starting point of an edge counts as negative, and end point of an edge counts as positive.

And
$$
H_0 (X) = Z_0 (X) / B_0 (X) = \mathrm{Span} \{ [a], [b], [d] \} \cong \mathbb{R}^3,  
$$
and here $ / $ is the group quotient operation, which means the output of the operation is what remains after regarding everything in the denominator as $ 0 $.
so in words, roughly we have:
$$
H_k (X) = \frac{k \text{-dimensional boundaryless \textit{elements} } }{\text{boudaries of }(k+1) \text{-dimensional \textit{elements} }}
$$
One remark on the word \textit{elements} in the above expression: the \textit{elements} are not always simplices, but they can be combinations of $ k $-simplices, such as $ a - b $. There are called the $ k $-chains, and they are simply formal sums of $ k$-simplices.

Now compute $ H_1 (X) $. There are $ 6 $ one dimensional simplices, but we only want to consider the boundaryless ones. None of them is boundaryless! But if we consider the $ 1 $-chains, we find the following boundaryless elements:

$$
\begin{eqnarray*}
e_{g d} + e_{d f} + e_{f g}, \\
e_{d e} + e_{e f} + e_{f d}, \\
e_{g d} + e_{d e} + e_{e f } + e_{ f g}
\end{eqnarray*}
$$

They generate $ Z_1 (X) $.

For $ B_1 (X) $, there is just one $ 2 $-simplices -- triangle $ \sigma_{d f g} $, and its boundary is $ e_{g d} + e_{d f} + e_{f g} $, and it generate $ B_1(X) $.

Quotient $ Z_1(X) $ by $ B_1(X) $, we have:

$$
\begin{eqnarray*}
e_{g d} + e_{d f} + e_{f g} \sim 0, \text{ i.e., it is quotiented out}, \\
e_{d e} + e_{e f} + e_{f d} \text{ stays the same},  \\
e_{g d} + e_{d e} + e_{e f } + e_{ f g} \sim e_{d e} + e_{e f} + e_{f d}
\end{eqnarray*}
$$

So $ H_1(X) = Z_1(X) / B_1(X) = \mathrm{Span} \{ e_{d e} + e_{e f} + e_{f d} \} \cong \mathbb{R}^1 $.

Since there is no higher dimensional simplex, the higher dimensional homology groups $ H_k (X) $'s are all trivial(i.e., they are $0 $-dimensional vector spaces)for $ k \geq 2 $.

The topological information that we learn from $ H_0(X) $ and $ H_1(X) $ is that: looking at $ 0 $ dimension, $ X $ essentially has $ 3 $ vertices that cannot be eliminated, and looking at $1 $ dimension, $ X $ essentially has $ 1 $ chain of edges that cannot be eliminated. If we look back at the example of the plane and punctured plane, we can approximate the plane by a simplical complex $ K_1 $ which is roughly a solid triangle, i.e. it has $ 3 $ vertices $a, b, c $, $ 3 $ edges $ e_{a b}, e_{b c}, e_{c a} $ and $ 1 $ triangle $ \sigma_{a b c} $. While the punctured plane can be approximiated by $ K_2 $ which is roughly an empty triangle, i.e., it has $ 3 $ vertices $ a', b', c' $, $ 3 $ edges $ e_{a' b'}, e_{b' c'}, e_{c' a'} $ and no triangle. Computing $ 1 $-dimensional homology groups for $ K_1 $ and $ K_2 $, we have $ H_1(K_1) \cong 0 $ and $ H_1(K_2) \cong \mathbb{R}^1 $. So they are topologically different.

Note the objects we are looking at are in a combinatorial nature and the maps are linear. These two features allows finite representation/storage of the objects and computations can be done using linear algebra.

The followings are a few related concepts. The dimensional of the vector space $ H_k(X) $ is called the $ k $-th Betti number of space $ X $, denoted by $ b_k $. Alternating sums of the Betti numbers is called the Euler characterisitic of $ X $, denoted by $ \chi (X) $. In this example,
\\[
\chi(X) = b_0 - b_1 + b_2 - b_3 + ... = 3 - 1 = 2.
\\]
