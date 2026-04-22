# My journey of completing Parallel Computing focus area in one semester

There are many things that spark my interest in HPC. The first instance was back when I was
in high school and doing high school project, I had a chance to work with molecular dynamic
simulation by using LAMMPS, and it was my first time I’ve heard about bunch of jargons such
as OPENMP and MPI and the fact that we can run the simulation in parallel. At that time, I also
had access to Mac Studio M1 (which was new at that time) with 64 cores. Despite having a lot of
core, I was the monkey that didn’t understand how to use these stuffs. So, I unfortunately cannot
effectively utilize the computing power as expected. 

> I swore to myself that one day, I would become a wise monkey.

After entering NUS, I didn’t have much passion with low-level programming at the start, but
CS2106, one of my favorite CS mods, really brought my passion back and led me to this point. I decided to challenge myself this semester: taking CS3210, CS3211, and CS4231 together in one semester. (with other CS
mods). Oh boy, this journey was extremely painful.

One thing that I once misunderstood about these mods is that all of these mods are related together. However, these mods have a pretty distinct flavour.

## CS3210 Parallel Computing
> Workload: 5/5, Enjoyability: 4/5 

Let's first address the elephant in the room. This mod is insane, heavier than CS2103T and deserves a 5/5 workload from me (I gave CS2103T 4.5/5). The examinable content itself is not that bad, since most of the stuff are straightforward, except coherence and consistency and GPU. However, the stuff you have to learn outside of the lecture (e.g. from tutorial, documentations, or your favourite AI chatbot) is a major timesink. I spent around 20 hours on A1, 15 hours on A2, and 10 hours on A3 (I soloed A1 and A2, paired with my friend for A3.) Most of the time spent were from trying to fix the code to pass benchmarks, collecting data (you have to collect data multiple times for sake of statistics), and writing reports of 4-5 pages. This is indeed a major timesink.

The marking for the assignment is somewhat strict, especially when you have to give sounded explanation for the result you got with supporting evidence. 

New things I have learned from assignments.

- OPENMP: SLURM, `perf`, `flamegraph`, `#pragma omp`
- GPU: mental model for GPU programming (which is very different from CPU), NVIDIA Nsight system, Nsight compute
- MPI: Ugly MPI syntax, deadlock will ruin your day. (sob :/), network latency is huge.

Overall, a solid module that teaches you a lot of stuff (especially outside lecture). Highly recommended, but prepare to lose your sanity.

## CS3211 Concurrent Programming
> Workload: 3/5 (around CS2106), Enjoyability: 5/5 

Unlike CS3210, the workload is pretty managable, but the content is a lot harder (especially memory model).

(TODO)


## CS4231 Parallel and Distributed Algorithms
> Workload: 1/5, Enjoyability: 4.5/5 

(TODO)

## Conclusion
> Average workload: 3/5, Enjoyability: 4.5

(TODO)
