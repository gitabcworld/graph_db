# Graph Database

Synthetic graph database generation. Each class is generated with a prototype and afterwards distortions are applied. To run the default example:

    $ pip install -r requirements.txt
    $ python generate_dataset.py
    
## Usage

Usage of __generate_dataset.py__ script:

```
usage: generate_dataset.py [-h] [--dirPrototypes DIRPROTOTYPES]
                           [--nodeThreshold NODETHRESHOLD]
                           [--dirDataset DIRDATASET] [--division DIVISION]
                           [--unbalanced] [--nodeDisplace NODEDISPLACE]
                           [--nodeAdd NODEADD] [--edgeMaximum EDGEMAXIMUM]
                           [--addEdge ADDEDGE] [--rmEdge RMEDGE]
                           [--edgeConnection EDGECONNECTION]

Generate a dataset from a given prototype folder.

optional arguments:
  -h, --help            show this help message and exit
  --dirPrototypes DIRPROTOTYPES
                        prototype folder
  --nodeThreshold NODETHRESHOLD
                        prototypes node threshold
  --dirDataset DIRDATASET
                        dataset folder
  --division DIVISION   division (tr, val, te)
  --unbalanced          Unbalanced database
  --nodeDisplace NODEDISPLACE
                        node std for distort its position
  --nodeAdd NODEADD     node std for adding a node in a source neighbourhood
  --edgeMaximum EDGEMAXIMUM
                        maximum number of new edges that can be added
  --addEdge ADDEDGE     probability to add new edge
  --rmEdge RMEDGE       probability to remove an edge
  --edgeConnection EDGECONNECTION
                        probability new edge is connected to an existing node
```

## Prototypes

Prototypes folder contains prototypes to generate different dataset and also combinations:

    $ --dirPrototypes ['./prototypes/Letters/', './prototypes/Digits/']

The  proposed prototypes can be found [here](./prototypes/README.md).

## Parameter discussion

Evaluation on the effect of the proposed parameters.

### Add nodes

Controlled by __--nodeThreshold__ parameter, increase the number of nodes of the prototypes before the deformation. It tries to add a node at the specified distance, equispaced following the edges.

Some examples with graph A normalized before and after adding the nodes:

|Original graph |
| ------------- |
|<img src="https://github.com/priba/graph_db/blob/master/readme_plots/A.png" width="200">|

| --nodeThreshold  | Image | | --nodeThreshold  | Image |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0.10  | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_01.png" width="200"> | | 0.20 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_02.png" width="200"> |
| 0.30  | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_03.png" width="200"> | | 0.40 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04.png" width="200"> |

### Node distortion

Controlled by __--nodeDisplace__ parameter, add random noise following a normal distribution center at each node with standard deviation set by --nodeDisplace.

Some examples with graph A where --nodeThreshold has been set to 0.40.

|Original graph |
| ------------- |
|<img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04.png" width="200">|

| --nodeDisplace  | Image | | --nodeDisplace  | Image |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0.01  | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_001.png" width="200"> | | 0.05 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_005.png" width="200"> |
| 0.10   | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01.png" width="200"> | | 0.20 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_02.png" width="200"> |

### Insert edges

Controlled by __--edgeMaximum__ parameter, __--addEdge__, __--edgeConnection__ and __--nodeAdd__, adds at most --edgeMaximum edges with probability --addEdge. The source node is always a existing node in the graph, the target node is an existing one with probability --edgeConnection. If a new node is add, it is created in a neighbourhood with standard deviation --nodeAdd.

Some examples with graph A where --nodeThreshold has been set to 0.40, --nodeDisplace 0.10, --edgeMaximum 10 and --nodeAdd 0.8.

|Original graph |
| ------------- |
|<img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01.png" width="200">|

| --addEdge  | --edgeConnection  | Image | | --addEdge  |  --edgeConnection  | Image |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| 0.05 | 0.75 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_005_075.png" width="200"> | | 0.05 | 0.50 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_005_05.png" width="200"> |
| 0.10 | 0.75 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_075.png" width="200">  | | 0.10 | 0.50 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_05.png" width="200">  |
| 0.25 | 0.75 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_025_075.png" width="200"> | | 0.25 | 0.50 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_025_05.png" width="200"> |
| 0.50 | 0.75 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_05_075.png" width="200">  | | 0.50 | 0.50 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_05_05.png" width="200">  |


### Remove edge

Controlled by __--rmEdge__ parameter, removes randomly edges with probability --rmEdge, however, at least one edge shall be kept.

Some examples with graph A where --nodeThreshold has been set to 0.40, --nodeDisplace 0.10, --edgeMaximum 10, --nodeAdd 0.8, --addEdge 0.1 and --edgeConnection 0.75.

|Original graph |
| ------------- |
|<img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_075.png" width="200">|

| --rmEdge  | Image | | --rmEdge  | Image |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0.01  | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_075_001.png" width="200"> | | 0.05 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_075_005.png" width="200"> |
| 0.10   | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_075_01.png" width="200"> | | 0.20 | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/A_04_01_01_075_02.png" width="200"> |


## Some Examples

Different levels of distortion for graph A with --nodeThreshold 0.4.

### LOW

* --nodeDisplace 0.05
* --nodeAdd 0.4
* --edgeMaximum 8
* --addEdge 0.1
* --rmEdge 0.05
* --edgeConnection 0.75

| Image | Image | Image |
| ------------- | ------------- | ------------- |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/0_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/1_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/2_A.png" width="200"> |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/3_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/4_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/5_A.png" width="200"> |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/6_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/7_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/LOW/8_A.png" width="200"> |

### MEDIUM

* --nodeDisplace 0.1
* --nodeAdd 0.5
* --edgeMaximum 10
* --addEdge 0.1
* --rmEdge 0.05
* --edgeConnection 0.6

| Image | Image | Image |
| ------------- | ------------- | ------------- |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/0_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/1_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/2_A.png" width="200"> |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/3_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/4_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/5_A.png" width="200"> |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/6_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/7_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/MED/8_A.png" width="200"> |

### HIGH

* --nodeDisplace 0.2
* --nodeAdd 0.8
* --edgeMaximum 10
* --addEdge 0.25
* --rmEdge 0.05
* --edgeConnection 0.6

| Image | Image | Image |
| ------------- | ------------- | ------------- |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/0_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/1_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/2_A.png" width="200"> |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/3_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/4_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/5_A.png" width="200"> |
| <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/6_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/7_A.png" width="200"> | <img src="https://github.com/priba/graph_db/blob/master/readme_plots/HIGH/8_A.png" width="200"> |

## Authors

* [Pau Riba](http://www.cvc.uab.es/people/priba/) ([@priba](https://github.com/priba))
* [Anjan Dutta](https://sites.google.com/site/2adutta/) ([@AnjanDutta](https://github.com/AnjanDutta))
