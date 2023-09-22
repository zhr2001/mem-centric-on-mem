## Motivation

* Insights

  - a larger number of feature extraction layers and a deeper hierarchy of features 
  - intermediate feature maps and workspace incur an order of magnitude higher memory usage compares to the weights of each layer
  - most of the intermediate data are concentrated on the feature extraction layers and are less significant in the later classify layers
  - the weights are mostly concentrated on the classifier layers 
  -  the per layer's memory usage is smaller then the mem required by the baseline policy

* implementation

  - static

    - vDNN_all

      > 将所有层进行优化，可以offload和prefetch的数据都进行相应操作

    - vDNN_conv

      > 只将计算时间占比最高的卷积层的数据进行offload与prefetch，来达到更好的训练速度

    - vDNN_all 的memory usage小于vDNN_conv,其训练速度慢于vDNN_conv

  - dyn

    > 插入一个profiling阶段对系统硬件参数建模实现一个balancing的效果

* evaluation

  - synchronize(streaming-computing waiting for streaming-memory)

    > forward阶段，在下一步计算之间应该等待将前一层数据offload到CPU上，backward阶段下一步求导之前应该等待将数据从CPU prefetch到GPU上，这其中内存操作与计算操作并行企图隐藏通信的开销，实际情况计算速度有可能快于内存存取，从而出现训练时间上的延迟，对于系统的训练效率产生影响

* disadvantage

  - performance loss because of synchronization
  - low feasibility because of the only strategy to convolution