# DENSIRED

## How to use

Use the following code to generate a skeleton, parameters are listed below

```
skeleton = datagen.densityDataGen()
```

Use the following code to obtain a dataset with *n* points from a skeleton

```
data = skeleton.generate_data(n)
datax = data[:,0:-1]
datay = data[:,-1]
```

To visualize the skeleton, call the following. For higher-dimensionalities, either specify *dcount* to get all pairs of the *dc* most spread out dimensions or specify the desired dimensions directly with *dims*.
```
skeleton.display_cores(dims=[d1,d2,...], dcount=dc)
```

To visualize a dataset, call the following. Give the dataset as *data*. The flags *show_radius* and *show_core* decide whether to display the core radii and core centers, respectively. For higher-dimensionalities, as with dispaly_cores, either specify *dcount* to get all pairs of the *dc* most spread out dimensions or specify the desired dimensions directly with *dims*.
```
skeleton.display_data(data, show_radius=False, show_core=False, dims=[d1,d2,...], dcount=dc)
```

## Skeleton Parameters
| **Parameter**        | **Abrev.** | **Default** | **Role**                                                                                                                                                                                                                                                                      |
|----------------------|------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dim                  | *dim*     | $2$           | dimensionality of the data set                                                                                                                                                                                                                                                |
| clunum               | *k*    | $2$           | number of clusters                                                                                                                                                                                                                                                            |
| core_num            | *cln*   | $10$          | number of cores across all clusters allows for explicit specification of core number per cluster                                                                                                                                                                              |
| domain_size         | *ds*   | $1$         | initial range of the starting position of the clusters                                                                                                                                                                                                                        |
| radius               | *r*   | $1$         | base range around core center where points can be generated                                                                                                                                                                                                                   |
| step                | $\delta$     | $1$         | base distance between cores                                                                                                                                                                                                                                                   |
| dens_factors       |  *cf*   | False       | cluster density/scale factors<be>multiplies both step size and radius for cluster<br> assigns all clusters a density factor 1 if False<br> assign random dens_factors *cf*$_{i}$ between $0.5$ and $2$ if True<br>can also be set per cluster in an array                                                                                                                  |
| momentum           | $\omega$     | $0.5$         | cluster momentum factors<br>assigns the value to all clusters as stickiness factor assigns<br>sets random stickiness factors between $0$ and $1$ if None<br>can also be set per cluster in an array                                                                                                                |
| min_dist            | *o*   | $1.1$       | overlap factor on the minimal distance between cores (sum of core sizes)                                                                                                                                                                                                              |
| step_spread | *w*     | $0$           | width of normal distribution on shift                                                                                                                                                                                                                                         |
| max\_retry           | *mr*  | $5$           | maximal number of attempts for generation before generation enters the failure handling                                                                                                                                                                                       |
| branch               | $\beta$   | $0.05$           | chance of creating a branch from the prior scaffolding of the cluster<br>done by restarting from a randomly selected core of the current cluster                                                                                                                                 |
| star                 |  $\varkappa$     | $0$           | star chance, chance of the initial starting core being chosen for any attempt of restarting applies to both failure and branching<br>remaining cores all have an equal probability                                                                                                                                                                    |
| seed                 |   *i*    | $0$         | seed for random operations                                                                                                                                                                                                                                                    |
| connections          | *cc*       | $0$         | number of connections randomly picks the specified number of connection pairs<br>allows for explicit specification of desired connections if a list is given<br>connections have to specified as "startid;endid"<br>only guarantees number of attempted connections, not final number |
| con_radius          | *r*$_c$    | $2$         | analogous to radius, used for connections                                                                                                                                                                                                                                     |
| con_step           | $\delta_c$  | $2$         | analogous to step, used for connections                                                                                                                                                                                                                                      |
| con_dens_factors  | *cf*$_c$ | False       | analogous to dens_factors, used for connections                                                                                                                                                                                                                             |
| con_momentum     | $\omega_c$  | $0.9$       | analogous to momentum, used for connections                                                                                                                                                                                                                                 |
| con_min_dist       | *o*$_c$  | $0.9$       | factor on the minimal distance between a connection and other cores                                                                                                                                                                                                           |
| clu\_ratios          | *ra*    | None        | distribution of data points across clusters                                                                                                                                                                                                                                   |
| min\_ratio           | *ra*$_m$   | 0           | minimal ratio of cores/points<br>(only used when these are randomly determined through *ra*, should be $<<1$)                                                                                                                                                                   |
| ratio\_noise         | *ra*$_n$     | $0$         | ratio of noise data points                                                                                                                                                                                                                                                    |
| ratio\_con           | *ra*$_c$    | $0$         | ratio of connection data points                                                                                                                                                                                                                                               |
| square               | $\square$       | False       | generate noise in square data space                                                                                                                                                                                                                                           |
| random_start               | *rs*       | False       | whether to start at a random position (True) or to start between two different clusters’ random cores (False)                                                                                                                                                                                                                                           |
| verbose              | *v*       | False       | Generate text outputs at key events                                                                                                                                                                                                                                           |
| safety               | *s*       | True        | activates various safety features, such as: noise generation only outside of two times core radius guarantee of at least a single point per core for data generation                                                                                                          |
## Data Generator Parameters
| **Parameter**        | **Abrev.** | **Default** | **Role**                                                                                                                                                                                                                                                                      |
|----------------------|------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| data_num          | *n*      |             | number of data points                                                                                                                                                      |
| non_zero          | *d_nz*       |     False        | guarantee at least one point per core                                                                                                                                                         |
| center         | *d_c*       |     False        | place first data point of core at center of core                                                                                                                                                         |
| seed         | *d_i*       |     None        | Updates generator seed<br>if None, maintains current data generator seed                                                                                                                                                 |

## DENSIRED for Streams
The core skeleton provided by DENSIRED allows for a setup for the stream data and offers user control over changes on a cluster and even a sub-cluster level. 
A simple continuous random sampling of new data points from all clusters according to the specified data ratios is, of course, possible. 
However, DENSIRED offers the user the ability to specify which clusters and even which cluster core to draw from.

To do so, the user has to specify a string parameter that describes the desired behavior of the data stream.
The generator supports one stream at a time and can be called as an iterator to get the next value from the stream. 
If the specification string is changed, the stream is reset back to the start.

The stream is split into specific blocks. These blocks are separated by "|" in the string. 
The stream iterator remains inside a block for a set amount of data points (either based on a default value or a specified number). 
The number of points to generate is specified by using a numeric value 𝑥 between braces "{x}".
Each block has a user-defined set of clusters and cores that the new data point can be drawn from. 

The clusters are set by their numeric identifier, separated from other clusters by "#". 
Within each cluster, individual cores can be specified. This is done by denoting the core identifiers inside square brackets "[x]". 
This supports a comma-separated list of individual identifiers, an end-inclusive range of identifiers denoted by a colon, as well as a combination of both.

By using regular brackets, it is possible to denote repetitions as well. These can be nested and their repetition number is designated with braces as with the point numbers for individual blocks. 
If no number is specified, the repetition count defaults to repeating the loop content once, meaning that the sequence will occur twice.
The exception to this is when the repetition without a repetition number is the final part of the string. In that case, the loop is instead repeated endlessly. If there is still a block at the end of the string after the last repetition, the stream stays in that block.
Should the final element be a repetition with a fixed count, the entire stream sequence is looped instead to maintain the repetition number.

Points are generated from the cores of the current block. The weights associated are drawn from the given parameters. 
The noise ratio is maintained if is added to a block to designate the section as a noisy one. 

If a connection is requested by calling its identifier, it also follows the connection ratio, with the ratio being split between all connections based on their core numbers during the block. 
The clusters are weighted according to the cluster ratios when disregarding any cluster that does not have a core in the block. There is no reduced weight for clusters with fewer cores.

To visualize the stream and to easily make adjustments, it is possible to request the data generator to display the behavior for the stream, which will provide a visualization of the skeleton of the block in each overall stream step.
It is especially advisable to display the stream behavior before generating the data when individual cores were chosen to ensure the desired cores were selected
