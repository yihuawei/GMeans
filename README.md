/*      gmeans-- Clustering with first variation and splitting
        Copyright(c) 2003 Yuqiang Guan (version 1.0)
*/

To compile: 
make

To remove .o and executable files:
make clean

To run:
gmeans [switches] matrix-file
        -a algorithm
           s: spherical k-means algorithm (default)
           e: euclidean k-means algorithm
           b: information bottleneck algorithm
	   k: kullback_leibler k-means algorithm
	   d: diametric k-means algorithm
	-c number-of-clusters
	-D delta, this float number is used as threshold for first-variations (default is 10E-6)
        -d dump the clustering process (default is false)
	-E number-of-kmeans-iterations, this integer sets when to use similarity estimation (default is 5)
        -e epsilon, this float number is used as threshold for k-means (default is 10E-3)
        -F matrix format
           s: sparse matrix in CCS (default)
           d: dense matrix i.e. each column represents a data point
           t: dense matrix transpose i.e. each row represents a data point
	-f number-of-first variations (default is 0)
        -i [S|s|p|r|f|c]
           initialization method:
              p -- random perturbation
              r -- random cluster ID
              f -- read from cluster ID seeding file
              S -- choose the 1st centroid the farthest point from the center of the whole data set, well separate all centroids (defualt)
              s -- same as 'S' but get the 1st centroid randomly
              c -- randomly pick vectors as centroids
	-L use Laplacian for Kullback_leibler distance measure (default is false) 
	      c alpha -- use prior
	      n       -- no prior 
        -O the name of output matrix (default is the prefix of the matrix file name)
	-P set prior values\n");
	      u -- uniform priors (default)
	      f filename -- read priors from a file
        -p perturb magnitude
	-S skip spherical K-Means, i.e. no k-means just first-variations (default is false)
        -s perturbation with a positive integer seed
        -T true label file
        -t scaling scheme (default is tfn)
        -V verify obj. func. (default is false)
        -v version-number
	-U cluster_file (to valuate a given clustering, default is false)
        -u upper bound of cluster number
	-W if you want word clusters too
        -w Omiga value is between 0 and -1

If you use sparse matrix, then 'word-doc-file' the prefix of 5 matrix file names (_dim, _col_ccs, _row_ccs, _tfn_nz and _docs); Otherwise if you use dense matrix (#word-by-#docs) then you should use '-F d' option or if you use dense matrix (#docs-by-#word), e.x. gene expression data, you should use '-F t' option; In either of these 2 cases the matrix file is 

#row #column
.............
.............
.

.............

first line gives the size of this rectangular matrix, then the matrix itself.

---------------------------------------------------------
   
The format of initial seeding file is

3893 1
0
0
.
.
.
2


where '3893' is the number of documents, '1' is for format purpose. 
Then the numbers in the column below are initial cluster ID for each document. 
If option 'c' is 3, for example, the cluster IDs should range from 0 to 2.

---------------------------------------------------------

The format of true label file is the same as initial seeding file. 

---------------------------------------------------------

The role of 'w' is to penalize on the number of clusters. Its value is 0 or negative. You may play with different 'w', say on Classic3 , setting a big upper bound on cluster number, to see when 'spmeans' stops what cluster number you get eventually. What 'w' gives you 3 clusters? Or if you get 5 clusters do they look good? So far we don't know what can be a good 'w' for a given data set yet. 
But maybe different cluster numbers are all OK if the clustering itself is good.

---------------------------------------------------------

Example of execution:

gmeans-linux -c 3 -T classic3_truelabel classic3
gmeans-linux -c 3 -a e -T classic3_truelabel -O classic3_output -f 20 classic3
gmeans-linux -c 3 -a b -T classic3_truelabel -i r classic3
gmeans-linux -c 3 -a k -L c 100000 -T classic3_truelabel -O classic3_output classic3

