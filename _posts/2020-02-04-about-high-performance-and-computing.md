---
title: "Fueling high performance in computing"
last_modified_at: 2020-02-04T20:44:00-05:00
categories:
  - Work
tags:
  - HPC
  - parallel
  - computing
  - speed
  - performance
  - history
---

<!--TODOs: revisit title, change category so that it appears at the top, edit the problem. Consider forking putting roofline model and work-depth analysis in a separate post-->
## Buying performance

Programming has always thrived on the use of abstractions. Someone who builds websites typically doesn't (and shouldn't) need to know about the changes in each of the generations of processor chips. Furthermore, just like buying more storage means you can save more data, buying a newer processor should mean you can run a single computation faster (without code re-write). Typically, a processor executes instructions in locksteps (or clock ticks). Increasing the speed of a processor (till the 80s) meant reducing this lockstep duration. This would hence make programs run faster without any code rewrite.

## Trouble in paradise

This wonderful cycle lost steam when it became extremely difficult to scale the clock speed any more. The problem was that the power used (and the heat produced) by these processors started increasing faster than their performance. 

Computer architects soon found a workaround to keep making processors faster. It was diabolically simple. To understand the solution, let us take a look at the brief organisation of a processor: 

<p align="center">
<img src="/assets/images/single_core.jpg"> 
</p>

A "Core" as shown in the image is the fundamental unit that does the required arithmetic computation(s). The aforementioned clock speed influences the speed of this core and hence the processor as a whole. Coming back to the problem, it was clear that one core could not get any faster. So, why not just add more cores to a single processors?

<p align="center">
<img src="/assets/images/multi_core.jpg"> 
</p>


Earlier, if you had scaled the clock speed of a single core by 2X, you would have needed (approximately) 4X power to run it. But instead, to run two cores you needed just 2X power. This hence gave them room to keep scaling the performance.

## Parallel computing to the rescue

With the advent of parallel processors however, it was suddenly impossible to simply "buy" performance. This architectural change had unprecedented far-reaching implications for everyone. The newfound concurrency was like quantum uncertainty to the deterministic universe of programs.
<!--High level languages (and their runtime systems) had to offer some libraries for exploiting this parallelism. -->
Several bugs came up due to parallelism in programs. You see, these cores could be given conflicting tasks to do, and any kind of checks at run-time would reduce performance. But lack thereof gave the programmer a loaded gun to (potentially) shoot himself with. Testing and debugging just got (and still is!) a whole lot trickier.

However, this was the easy part. You see, even theoretical computer science had to play catch-up with this architectural change. 
Regardless of what the local Apple "Genius" says, it is a fallacy to always expect linear scaling--with respect to the number of cores--for any given algorithm. 

Earlier, theoretical analysis of an algorithm was done with the assumption that a hardware change will affect all algorithms equally. 
Suddenly, this was not true! Some algorithms could work better on parallel processors than others. I'll try to explain this with the following example.


### Thinking fast and slow

<!--Consider this problem: -->

We have a grid of 5 x 5. Imagine that each cell in the grid is an independent kingdom. You are a conqueror, and want to rule the 25 kingdoms and sit on the beyond-iron throne (a sustainable replacement for iron, made out of leaves and twigs, but looks just like iron).

|---|---|---|---|---|
| &nbsp; &nbsp;   | &nbsp; &nbsp;   |&nbsp; &nbsp; | &nbsp; &nbsp;   |&nbsp; &nbsp;    |
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |

However, let's imagine you are working alone. You can only capture one kingdom per day (no one can defeat you), and hence grow your territory. A territory is just a group of kingdoms that share a wall with each other (adjacent cells) and are controlled by the same king.

So, how many days would it take you for world domination?

Well, obviously 25. As shown below, you can start at the top left corner and conquer kingdoms as you go left to right and top to bottom. Or you could start at the center and work your way outwards. All the possible sequences of moves are equivalent in this case.

|----|----|----|----|----|
| 1  | 2  | 3  | 4  | 5  |
| 6  | 7  | 8  | 9  | 10 |
| 11 | 12 | 13 | 14 | 15 |
| 16 | 17 | 18 | 19 | 20 |
| 21 | 22 | 23 | 24 | 25 |

In an alternate scenario, let's imagine you are the leader of the unsullied. You basically command an _infinite_ set of people, all of whom will fight for you, and needless to say, always win.

But here's the catch. On each day, you can either choose to (all by yourself) conquer a new kingdom that does not share a wall with your current territory (and thereby making a new territory with a single kingdom), OR, you can use your men to annex all kingdoms that are adjacent to any one of your current territory(ies).

#### For example:
Let's say, at the end of a day you have two territories, one on the top left end of the grid and one at the bottom right end as shown:

|---|---|---|---|---|
| X | X |&nbsp; &nbsp; |   |   |
| X |   |   |   |   |
|   |   |   |   |   |
|   |   |   | X | X |
|   |   |   |   | X |


The next day, you can either grow the territory at the _bottom right end_, or at the _top left end_ or venture out and make a _new territory_ with a single kingdom. The three possibilities are shown in the same order:


|---|---|---|---|---|
| X | X |   |   |   |
| X |   |   |   |   |
|   |   |   | X | X |
|   |   | X | X | X |
|   |   |   | X | X |


|---|---|---|---|---|
| X | X | X |   |   |
| X | X |   |   |   |
| X |   |   |   |   |
|   |   |   | X | X |
|   |   |   |   | X |


|---|---|---|---|---|
| X | X | &nbsp; &nbsp;   |   |   |
| X |   |   |   |   |
|   |   |   |   |   |
|   |   |   | X | X |
| X |   |   |   | X |

Now, what is the fastest way to rule the 25 kingdoms? Well, just take the center kingdom and keep growing this territory every day. Easy.


|---|---|---|---|---|
| 5 | 4 | 3 | 4 | 5 |
| 4 | 3 | 2 | 3 | 4 |
| 3 | 2 | 1 | 2 | 3 |
| 4 | 3 | 2 | 3 | 4 |
| 5 | 4 | 3 | 4 | 5 |

The above grid shows you at which day you will get the respective kingdoms. On day 5, you can sit on the beyond-iron throne!


However, not all strategies are so fast. Consider a bad start in a corner, this is what will happen:

|---|---|---|---|---|
| 3 | 2 | 3 | 4 | 5 |
| 2 | 1 | 2 | 3 | 4 |
| 3 | 2 | 3 | 4 | 5 |
| 4 | 3 | 4 | 5 | 6 |
| 5 | 4 | 5 | 6 | 7 |

So now you see that some algorithms can exploit parallelism better than others.
Their run-time in the sequential setting was the same, but some got faster than others when we added parallelism. As I added parallelism to the system, I also added some constraints. Not everything could be parallelised. 

This maps to computers quite accurately. Only arithmetic computations were accelerated by this newfound parallelism. Disk access, network communication, or even RAM access in some cases was not parallel. Only one core could us these resources at a given time. 

## The roofline model

In order to have realistic expectations from faster processors for any given algorithm, the architects proposed a nice performance model based on the above assumption.  To understand it, let's define some jargon first. 

#### FLOPs
The multiplication of two floating point (decimal) numbers followed by an addition with a third floating point is collectively measured as 1 FLoating point OPeration, or __1 FLOP__. We can hence define a speed metric, __FLOPs__ (it is not the plural of FLOP), as the number of floating point operations a core can do in 1 second. 

#### Bandwidth
The speed of RAM access is called memory bandwidth. It simply refers to the number of bytes you can read per unit time.

Coming back to the model, it quantifies the scalability of an algorithm with a new metric, __arithmetic intensity__. This is measured as the number of floating point operations done per byte of data read.

Now, for a given peak bandwidth <img src="https://render.githubusercontent.com/render/math?math=\color{white}B" height="3%" width="3%"> and a peak FLOPs rate <img src="https://render.githubusercontent.com/render/math?math=\color{white}P" height="3%" width="3%"> of a processor, the speed of a program is given by:

<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=\color{white}S=\min%20{\begin{cases}P \\B \times I\end{cases}}" height="40%" width="40%"> 
</p>

The intuition behind this is the same as before. Either the implementation of the program is compute heavy, or memory heavy. In case it is compute heavy, then the speed of the processor is the only thing that influences the run-time of the program implementation. If not, then the memory bandwidth must be the bottleneck.
The graph shows the two cases:

<p align="center">
<img src="/assets/images/roofline.jpg"> 
</p>

To make the most of a parallel processor, you need to be in the right hand region of this curve. 




<!--TODO add work-depth analysis and roofline model-->
<!--Do not delete:-->
<!--Theoretical analysis of run-time of an algorithm conventionally had just one variable: input size. Comparison of two algorithms was done by how much time would it take to process an input of a given size. Since all algorithms would have benefited equally from a clock speed increase, such an analysis was sufficient. -->
<!--It was clear that going forward, core count of the processors would keep increasing. A generic (lasting) analysis of algorithms would have to account for this change. To put in another way, the theoretical analysis had to allow for an easy speed comparison across different machines (with different number of cores) for the same algorithm. Therefore, the analysis was now multivariate, with the new variable being number of cores! -->

<!--//Describe in brief how to argue for speed in a parallel setting. Amdahl, Gustafson, Work-Depth, Roofline etc.-->

