# Density-Based Spatial Clustering of Applications with Noise (DBSCAN)
Is an unsupervised density-based clustering algorithm. Density-based means that
the algorithm focuses on the distance between each point and it's neighbors
instead of the distance to a centroid like K-Means. One way to describe DBSCAN
is:
> given a set of points in some space, it groups together points that are closely packed together 
(points with many nearby neighbors), marking as outliers points that lie alone in low-density regions 
(whose nearest neighbors are too far away). [Wiki](https://en.wikipedia.org/wiki/DBSCAN)

One thing that is interesting is that we can define what _close_ means by using
 different distance functions. For this tutorial we'll use 
 [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance),
which is basically the normal way we measure distance.
Another way to say how DBSCAN works is it finds central or core points and finds
which points are connected to those core points. All points reachable by the
core points are clustered together. A point is a core point it has some minimum
amount of neighbors within a given radius. A point _q_ is reachable from _p_ if
there is a path p<sub>1</sub>, ..., p<sub>n</sub> with p<sub>1</sub> = p and
p<sub>n</sub> = q,
where each p<sub>i+1</sub> is directly reachable from p<sub>i</sub> (all the
points on the path must be core points, with the possible exception of q).
The following visual shows core points __A__ with reachable points
__B__ & __C__. The point __N__ is considered an outlier.

<div style="width:402px; font-size: 94%; text-align: center;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/a/af/DBSCAN-Illustration.svg" alt="wikimedia_DBSCAN" style="width:400; height: 288;">
</div>

You may be wondering why
 we care since everyone already knows K-Means.
 Well, The DBSCAN algorithm views clusters as areas of high density separated
 by areas of low density.
 Due to this rather generic view,
clusters found by DBSCAN can be any shape, as opposed to k-means which
assumes that clusters are convex shaped.
So we gain a little bit more flexability. Below we'll see some examples of how
DBSCAN outperforms K-Means.

## Algorithm Overview 
When constructing the DBSCAN algorithm we need four things: 
  1. Minimum amount of points (minPts) 
  2. Distance function (dist\_func) 
  3. Distance from each point (Eps) 
  4. data 
 
> ASIDE: Eps stands for epsilon, the greek letter, which is used in mathematics 
> to represent a region around an object. An arbitrary but common choice.  
 
To perform DBSCAN we'll have to check each point and count its neighbors within  
the given distance.  Then we'll drop all points that don't have at least minPts  
neighbors. Now that we have our core points we'll have to find all reachable  
points for each core point to create our clusters.  
 
## Writing the algorithm 
To write this algorithm I'll be using Python 3.6 and Numpy 1.15.  
 
## Examples 
Sci-Kit Learn has a built-in DBSCAN which we can use to test out some examples  
and compare to our implimitation written above. 
 
## Caveats 
When DBSCAN doesn't work 
OPTICS 
 
## Further reading 

## Bibliography
  - Figure 1 By Chire [CC BY-SA 3.0  (https://creativecommons.org/licenses/by-SA/3.0)], from Wikimedia Commons
