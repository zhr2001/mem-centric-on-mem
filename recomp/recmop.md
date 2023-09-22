## Motivation

- the size of the parameters are relatively small comparing to the size of
  the intermediate feature maps in many common deep architectures
- more general, not only for convolution layer(contrary to vDNN)

## Mem Optimization

- In place operation
  - the data flow graph must be constructed because of data dependencies
- Memory Sharing
  - liveness of memory should be considered 

## Trade Computation & MEM

- Drop intermediate result of low-cost operators & Keep time-consuming operators' result
- Divided the whole graph into subgraph segments,keep the output of each segment and drop the internal result of each segment and recompute this result when bp
- Use greedy algorithm to construct sub-graph

## Evaluation

- 30% computation cost(double forwarding computation) for O(\sqrt(n)) MEM cost