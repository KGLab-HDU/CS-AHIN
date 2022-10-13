<div align='center' ><font size= 6>Efficienct and Effective (k,P)-Core Based Community Search over Attributed Heterogeneous Information Networks</font></div>

## 1 Introduction

This is a description of the code used for the experiments described in the paper titled *Efficienct and Effective (k,P)-Core Based Community Search over Attributed Heterogeneous Information Networks. The code is available at [(4open.science)](https://anonymous.4open.science/r/Efficienct-and-Effective-k-P--Core-Based-Community-Search-over-Heterogeneous-Attributed-Graphs-B2F9/).

## 2 Algorithms

We evaluate ten algorithms (including 7 algorithms presented by this paper) as follows.

| ALGORITHEM   | SOURCE                                                       |
| ------------ | ------------------------------------------------------------ |
| ATC          | [VLDB' 2017](https://dl.acm.org/doi/abs/10.14778/3099622.3099626) |
| ACQ          | [VLDB' 2015](http://www.vldb.org/pvldb/vol9/p1233-fang.pdf)  |
| VAC          | [ICDE' 2020](https://ieeexplore.ieee.org/abstract/document/9101813) |
| Exact        | Sec-4                                                        |
| AP-MCD       | Sec-5.2                                                      |
| AP-BD        | Sec-5.3                                                      |
| AP-BDP       | Sec-5.4                                                      |
| AP-MCD-Index | Sec-5.2 + Sec-6                                              |
| AP-BD-Index  | Sec-5.3 + Sec-6                                              |
| AP-BDP-Index | Sec-5.4 + Sec-6                                              |

## 3 Requirements

The experiments have been run on an Ububtu Linux computer with a 3.7 GHz Xeon processor and 128GB RAM. All online algorithms are written with `java1.8`,and the PG-Index are written in C++ and make use of the Boost C++ libraries, OpenMP parallel instructions.

## 4 Datasets

Our experiment involves four Datasets,  [DBLP-small](https://drive.google.com/drive/folders/1M_JF2KK0EmgJiPSXg2Mylwsc4H_-KHcb), [DBLP](https://drive.google.com/drive/folders/1N3aTcTqD3Vy5d-agfqPahHA4V1AP81ip), [IMDB](https://drive.google.com/drive/folders/1oeB6uBSAkUtF5llFBfmKoROBvqAyV_um), [Foursquare](https://drive.google.com/drive/folders/1lIBq-3j4J2DW0wjtHfqCBMjTQwpSx6cM) ,each one consists of  following files:

- vertex.txt
  Each line represents a vertex in DBLP. Each line starts with the vertex_id, following by vertex type.

- edge.txt
  Each line represents an edge in DBLP. Each line starts with the edge_id, following by edge type.

- graph.txt
  Each line represents an adjacent array. Each line starts with the vertex_id, following by a list of neighbor_vertex_id and edge_id.

- attribute.txt

  Each line represents the attribute of one node. Each line starts with the vertex_id, following by a list of attributes.

**Example of the dataset (DBLP)**

- vertex.txt

| vertex_id | vertex type |
| :-------: | :---------: |
|     0     |      0      |
- edge.txt

| edge_id | edge type |
| :-----: | :-------: |
|    0    |     0     |
- graph.txt

| vertex_id | neibor1_id | edge1_id | neibor2_id | edge2_id |
| :-------: | :--------: | :------: | :--------: | :------: |
|     0     |   140828   |    0     |   203893   |    1     |
- attribute.txt

| vertex_id | attribute1 | attribute2 | attribute3 |
| :-------: | :--------: | :--------: | :--------: |
|     0     |     0      |     1      |     1      |

## 5 Usage

### 5.1 Our community search methods

Our proposed method, with a specific *query node* (\<queryid>), a *query k* (\<queryK>), a batch delete size *m* (\<querym>), a coverage threshold \<$\tau$> and a meta-path (\<metapath>), can be run using the following command:

```
Java -cp MCSH.jar MCSH.online.Exact <dataset> <queryid> <queryK>
Java -cp MCSH.jar MCSH.online.Asearch <dataset> <queryid> <queryK> <querym> <$\tau$> <metapath>
```

For the query example

```
Java -cp MCSH.jar MCSH.online.Asearch DBLP_small 511 5 2 0.2 0,1,0;0,0
```

Output

For our method, we output the query conditions and the result of query, inlcuding the results for AP-MCD, AP-BD, and AP-BDP. 

```
MCD: queryId:511,queryK:5,queryMPath:0-0-1-0-0,0.2
find:17
704 642 931 291 70 487 775 938 876 751 883 184 26 92 382 511 319
BD: queryId:511,queryK:5,queryMPath:0-0-1-0-0,tao,0.2,querym:2
find:17
704 642 931 291 70 487 775 938 876 751 883 184 26 92 382 511 319
BDP: queryId:511,queryK:5,queryMPath:0-0-1-0-0,tao,0.2,querym:2
find:17
704 642 931 291 70 487 775 938 876 751 883 184 26 92 382 511 319 
```

### 5.2 Index construction

Before building index, you should compile dynamic library on Linux system. 

Then, you can run the following instructions for build graph index.

```
$ cd MA-NSW
$ make
$ cd tests/test/
$ make
```

After the build is completed,  you can build and search the PG-index through the jar package.

For index build, we can run using the following command:

```
Java -cp MCSH.jar MCSH.online.IBBuild <dataset> <metapath> <neighbors number 𝑟> <$\lambda$>
```

For the build example:

```
Java -cp MCSH.jar MCSH.online.IBBuild DBLP-small 0,1,0;0,0 10 0.2
```

### 5.3 Index-based community search

Our proposed method, with a specific querynode, a queryk, a batch delete size m, a $\tau$ and a meta-path, can be run using the following command:

```
Java -cp MCSH.jar MCSH.online.IBAlgorithm <dataset> <queryid> <queryK> <querym> <$\tau$> <metapath> <n> <$\lambda$>
```

For the query example

```
Java -cp MCSH.jar MCSH.online.IBAlgorithm DBLP_small 511 5 2 0.2 0,1,0;0,0 30 0.2
```

Output

For index search algorithms, we output the query conditions and the result of query, inlcuding the results for AP-MCD-Index, AP-BD-Index, and AP-BDP-Index. 

```
queryId:511,queryK:5,queryMPath:0-0-1-0-0,tao:0.2,n:30,lambda:0.2
MCD-index: find:17
704 642 931 291 70 487 775 938 876 751 883 184 26 92 382 511 319
BD: find:17
704 642 931 291 70 487 775 938 876 751 883 184 26 92 382 511 319
BDP: find:17
704 642 931 291 70 487 775 938 876 751 883 184 26 92 382 511 319 
```

### 5.4 Comparing algorithms

For the compare algorithms, we can run using the following command separately:

```
Java -cp MCSH.jar MCSH.compare.VACSearch <dataset> <queryid> <queryK> <metapath>
Java -cp MCSH.jar MCSH.compare.ATCSearch <dataset> <queryid> <queryK> <metapath>
Java -cp ACQ.jar hku.algo.ACQSearch <dataset> <queryid> <queryK> <metapath>
```

For example:

```
Java -cp MCSH.jar MCSH.compare.VACSearch DBLP_small 511 5 <0,1,0;3,0>
```

Output

For these algorithms, we output the query conditions and the result of query. 

```
VAC: queryId:511,queryK:5,queryMPath:0-0-1-0-0
find:13
704 385 802 614 775 487 393 938 779 14 751 92 511
```

