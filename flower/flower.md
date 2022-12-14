# Benchmark QUBO models for digital halftoning

- QUBO models are generated from gray scale images flower.png and flower-k.png (256x256 pixels).
- The original gray scale image was downloaded from unsplash.com, which offers free photos for commercial and non-commercial purposes.
- Two QUBO models:
  - **flower**: a 65536-bit QUBO model with unknown optimal solution. Problem files: flower.json.gz/flower.mm.gz   (two files store the same QUBO model)
  - **flower-k**: a 65536-bit QUBO model with a known optimal solution.
    Problem files: flower.json.gz/flower.mm.gz   (two files store the same QUBO model)
    Optimal solution: -225466781
* These QUBO models are designed to obtain binary images that reporduce flower.png/flower-k.png.
* By arranging 65536-bit solution in a 256x256 matrix such that 0/1 are black/white, we can obtain such binary images.


# QUBO (Quadratic Unconstrained Binary Optimization) problem and QUBO model
* An $n$-bit **QUBO model** is defined by an $n\times n$ upper triangular matrix $W=(W_{i,j})$ $(0\leq i\leq j\leq n-1)$.
* For an $n$-bit binary vector $X=(x_i)$ $(0\leq i\leq n-1)$, the energy of $X$ is $E(X)=\sum_{0\leq i\leq j\leq n-1}W_{i,j}x_ix_j$.
* **QUBO problem** aims to find a binary vector $X$ that minimizes the energy $E(X)$.
* A QUBO model defined above is **base 0**, because the index starts from 0. On the other hand, a QUBO mode can be **base 1** if the minimum index is 1 such that $W=(W_{i,j})$ $(1\leq i\leq j\leq n)$, $X=(x_i)$ $(1\leq i\leq n)$, and $E(X)=\sum_{1\leq i\leq j\leq n}W_{i,j}x_ix_j$

## QUBO model and QUBO problem
* Example of an QUBO model
$
W=
\begin{pmatrix} 
2 & 2 & -3& 0& 4&\\ 
 & 1 & -2& -2& 0&\\ 
 &  & -2& -4& 0&\\ 
 &\text{\huge{0}}  & & 5& 1&\\ 
 &  & & & -4& 
\end{pmatrix}
$
* The optimal solution is $X=[0,1,1,0,1]$, which takes $E(X)=-7$.

# File formats for QUBO models

## JSON format
* JSON format has three keys.
  
| key    | value                                                      |
| ------ | ---------------------------------------------------------- |
| "nbit" | bit count of QUBO model.                                   |
| "base" | base of QUBO model. 0 or 1. Zeor(0) is assumed if omitted. |
| "qubo" | list of non-zero elements of $W$                           |

* Non-zero elements of "qubo" is $[i,j,W_{i,j}]$ $(base\leq i\leq j<nbit+base)$.
* Example of a QUBO model in JSON format
```JSON
{
  "nbit":5,
  "base":0,
  "qubo":[
    [0,0,2],
    [0,1,2],
    [0,2,-3],
    [0,4,4],
    [1,1,1],
    [1,2,-2],
    [1,3,-2],
    [2,2,-2],
    [2,3,-4],
    [3,3,5],
    [3,4,1],
    [4,4,-4]
  ]
}
```
* The file extension must be .json or .json.gz (if compressed by gzip).

## Matrix Market file format
* Text file.
* The base must be 1.
* The first line must have four numbers: (1) bit count $n$ of QUBO model, (2) non-zero element count of $W=(W_{i,j})$, (3) the minimum of $W_{i,j}$, and (4) the maximum of $W_{i,j}$.
* The following lines have three numbers: $i$, $j$, and $W_{i,j}$ separated by space characters.
* Example of a QUBO model in Matrix Market format.
```TEXT
5 12 -4 5
1 1 2
1 2 2
1 3 -3
1 5 4
2 2 1
2 3 -2
2 4 -2
3 3 -2
3 4 -4
4 4 5
4 5 1
5 5 -4
```
* File extension must be .mm or .mm.gz (if compressed by gzip).

# Files
* flower.md: This file
* flower.png: original gray scale image
* flower.json.gz: QUBO model (JSON) for digital halftoning of flower.png
* flower.mm.gz: QUBO model (Matrix Market) for digital halftoning of flower.png
* flower-k.png: original gray scale image
* flower-k.json.gz: QUBO model (JSON) for digital halftoning of flower-k.png
* flower-k.mm.gz: QUBO model (Matrix Market) for digital halftoning of flower-k.png
* flower-k-optimal.json: Optimal solution for **flower-k**
* flower-k-optimal.png: binary image generated from solution **flower-k-ptimal.json**
* Optimal solution for **flower-k** is -225466781, while that for **flower** is unknown.

# Statistics
* Statistics of two QUBO models.
  
|          | flower      | flower-k    | description                                  |
| -------- | ----------- | ----------- | -------------------------------------------- |
| nbit     | 65536       | 65536       | bit count                                    |
| nedge    | 4106120     | 4106120     | edge count of QUBO graph                     |
| min_deg  | 38          | 38          | minimum degree of QUBO graph                 |
| max_deg  | 128         | 128         | maximum degree of QUBO graph                 |
| ave_deg  | 125.3088379 | 125.3088379 | average degree of QUBO graph                 |
| density  | 0.00191209  | 0.00191209  | density of QUBO graph                        |
| diameter | 64          | 64          | diameter of QUBO graph                       |
| min_val  | -123113     | -126077     | minimum value of elements in $W$             |
| max_val  | 6500        | 6500        | maximum value of elements in $W$             |
| offset   | 227848577   | 225466781   | offset to obtain total error of binary image |

* We can consider that, for a QUBO model $W=(W_{i,j})$, $W$ is the weight matrix of an weighted undirected graph called **QUBO graph**, such that
  * QUBO graph has nbit nodes,
  * two nodes $i$ and $j$ is connected with by an edge with weight $W_{i,j}$ if $W_{i,j}$ is non-zero.
  * $W_{i,i}$ is the weight of node $i$.
* Statistic above includes that for the QUBO graphs for QUBO models.