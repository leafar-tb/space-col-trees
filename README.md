# space-col-trees
This study project implements the methods presentde in the paper 'Modeling Trees with a Space Colonization Algorithm' by Adam Runions, Brendan Lane, and Przemyslaw Prusinkiewicz (found [here](https://www.researchgate.net/publication/221314843_Modeling_Trees_with_a_Space_Colonization_Algorithm) or [here](http://www.algorithmicbotany.org/papers/colonization.egwnp2007.html)).
In this implementation the shape of the resulting trees (or shrubs) can be defined by any mesh. This leads to interesting opportunities for digital topiary.
<p align="center"><img src="https://raw.githubusercontent.com/leafar-tb/space-col-trees/master/images/bunny.png"></p>
<p align="center"><img src="https://raw.githubusercontent.com/leafar-tb/space-col-trees/master/images/dragon.png"></p>

## Extension: Wind
The paper introduces a bias parameter to influence the growth direction of trees.
Vertical bias produces satisfying results, with branches bending down with or striving up against gravity.
Horizontal bias also creates interesting results, but whereas one might expect wind-like effects from a horizontal influence, comparison with [nature](http://scruss.com/talks/02006/bcs/pics/vestas-tree.jpg) shows little similarity.
One reason for this is that bias is a local effect.
It influences the individual growth steps, but not the branch or tree as a whole.
In particular, the shape of the crown as befined by the input mesh will be retained.
Shaping and placement of the input mesh can improve the result, but allows little control over the bending of branches.
Also, the tree ends up growing against the bias or perpendicular to it.
For wind, however, the tree would grow in its direction.
<p align="center">
<img src="https://raw.githubusercontent.com/leafar-tb/space-col-trees/master/images/gBias-hor.png"> </br>
Trees generated from a sphere under the influence of bias
</p>

The approach uesd here for creating a more global wind effect on the tree is to modify the attraction points, rather than the nodes.
All attraction points are translated every iteration by a user defined wind vector.
This leads to a slow shift of the growth directions over time, creating smoothly bending branches.
As can be seen below, the results are much closer to nature, despite also using a sphere as crown shape.
(Note also, that bias above and wind below were applied in the same direction.)
Applying 'wind' vertically, is also an interesting extension of the effects already achievable through bias.

<p align="center">
<img src="https://raw.githubusercontent.com/leafar-tb/space-col-trees/master/images/wind-hor.png"></br>
Trees generated from a sphere under the influence of 'wind'
</p>

## Usage
You need to have [NumPy](https://www.scipy.org/scipylib/download.html) installed, but otherwise this should work like any other Blender add-on.
Two operators are added to Blender:

**Generate Tree**:
The mesh you have selected will be used as shape for tree's crown.
First you need to select the number of trees you wish to grow into that shape.
(Some might fail to grow, if e.g. they are to far away.)
Then you can place the roots and configure the other parameters like density, wind, etc.
The result is a skeletal representation of the tree(s) as edges.

**Mesh Tree**:
This operator works on tree skeletons as produced by 'Generate Tree'.
You can prune the skeleton beforehand.
Other editing actions or building a skeleton by hand may not work as expected.
See Skeleton.fromBlenderSkeleton() for more details.
The result is a tree mesh (with leaves, if desired).
