---
title: whoami
layout: archive
classes: wide
author_profile: true
color: Gray
---

I am a PhD student at the University of Utah, advised by Prof. [Saday](https://www.cs.utah.edu/~saday/) Sadayappan.
My research interest lies broadly in High Performance Computing (HPC).
In the past, I have worked with IBM Research in New Delhi, towards making AI faster.
As a student, I did some research in languages and middleware for HPC at [ETH Zurich](https://spcl.inf.ethz.ch/) and [Inria](https://www.inria.fr/en/teams/datamove).
I went to college at the Birla Institute of Technology and Science [(BITS)](https://www.bits-pilani.ac.in/) in a small village called Pilani, in India.
While powerlifting gives me my daily adrenaline fix, I like to hike and ski when on a vacation.

### Research interests
Currently, I am working on accelerating sparse tensor decompositions in both shared and distributed memory settings.
Overall this problem is vastly multidimensional (no pun intended), primarily because the data (non-zero values) are not always structured.
Therefore, load imbalance and high data movement make performance optimisation hard.
<!-- make this a blog !-->
<!--Parallel performance is therefore strangled by load imbalance in work distribuion and/or data movement.  Accelerating iterative decomposition algorithms for example CP or Tucker decomposition begins with some fairly non-trivial preprocessing.  This impacts the load balance when each iteration is parallelised, and also the amount of data moved across memory hierarchies.  In each iteration, each dimension of the tensor is compressed to a smaller expanse using some form of a matrix multiply a matricised tensor product, to be more specific.  To make each iteration faster, an optimal schedule has to be pre-computed. This depends not only on the amount of memory at each level of the hierarchy, but also on the shape and sparsity pattern of the tensor.  An optimal schedule in-fact affects the total number of FLOPs, the parallelism depth, to be more precise and even the number of cache misses.  The most recent research shows that partially decomposed tensors can be saved in memory, in order to reduce the FLOP count in each iteration of the decomposition.  Interestingly enough, wall-time measurements do not always agree with this hypothesis.  This is because, the intermediate results can be less sparse, and increase the pressure on the memory.  It might therefore, be faster to just keep re-computing the same contractions instead of storing them!  Finally, some nice engineering choices have to be made while actually writing this out in C.  Walltime, at the end of the day, does not care about your model!-->
I see tremendous applications of this research in several domains, including large scale training of neural networks, simulations of quantum circuits and computational physics and chemistry.
Towards the end of my PhD, I aim to work on tailoring some performance optimisation techniques into these domain specific workloads.


<!-- add links and papers!-->
In the past, I have enjoyed working on:
* model pruning for NLP and graph neural nets
* Kvik: task splitting middleware for the Rust language
* DACE: a domain specific lanugage that enables dataflow-based performance optimisation
* flight software for a nanosatellite!



### Updates
* *July, 2020: Our [paper](http://proceedings.mlr.press/v119/goyal20a/goyal20a.pdf) on making the BERT model (in natural language processing) faster was presented at ICML' 20.*
* *February, 2021: My first patent application (as the primary inventor) filed by IBM Research with the USPTO.*
* *June, 2021: Got the Outstanding Technical Achievement Award at IBM Research worldwide.*
* *July, 2021: My 4th patent application filed by IBM Research with the USPTO.*
* *March, 2021: Received a PhD offer from the University of Utah, under Prof. Sadayappan! Looking forward to joining in Fall' 21.*
* *August, 2021: Officially a student again! Super excited to work on accelerating sparse computations.*
