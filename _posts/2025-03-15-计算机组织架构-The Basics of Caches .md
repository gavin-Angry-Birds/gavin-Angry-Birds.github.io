---
layout:     post
title:      计算机组织架构
subtitle:    "\" Computer Organization and Design:Hardware/
Software Interface,\""
date:       2025-03-15
author:     Gavin
header-img: img/64ef35890cb90_computer_organization_and_architecture_01.png
catalog: true
tags:
    - blog
---



# The Basics of Caches 

> Larger blocks exploit spatial locality to lower miss rates. As Figure 5.11 shows, increasing the block size usually decreases the miss rate. Th e miss rate may go up eventually if the block size becomes a signifi cant fraction of the cache size, because the number of blocks that can be held in the cache will become small, and there will be a great deal of competition for those blocks. As a result, a block will be bumped out of the cache before many of its words are accessed. Stated alternatively, spatial locality among the words in a block decreases with a very large block; consequently, the benefi ts in the miss rate become smaller. 


![](/image/cache1.png)



## why “Spatial locality among the words in a block decreases with a very large block”

### 1. **空间局部性（Spatial Locality）的基本概念**
空间局部性指的是**程序访问的内存地址通常是彼此接近的**，例如数组遍历、顺序执行的指令流等。为了利用空间局部性，缓存（Cache）通常使用较大的块（Block），这样当一个地址被访问时，整个块都会被加载到缓存中，从而在后续访问相邻地址时命中缓存，提高效率。

### 2. **为什么块变得“过大”时，空间局部性会降低？**
当块大小较小时，加载进缓存的内容往往都是程序即将访问的地址范围，因此可以充分利用空间局部性。但如果块大小过大，会出现以下问题：

1. **程序的访问模式决定了真正有效的数据量**：
   - 典型的程序访问模式往往是**局部的**，例如数组操作时，通常访问的是相邻的少量元素，而不是整个大块的数据。
   - 如果块过大，虽然会加载更多数据，但这些数据可能在短时间内不会被访问，导致实际利用率下降。

2. **Cache 资源有限，块变大导致块数减少**：
   - 例如，假设缓存总大小是 64 KB，如果块大小是 16 B，则可以存储 4096 个块；
   - 但如果块大小增加到 4 KB，则只能存储 16 个块；
   - 这样，**同样大小的缓存只能存放更少的块，导致块之间竞争加剧，缓存替换频繁**，使得数据还未被充分利用就被淘汰。

3. **空间局部性的影响范围有限**：
   - 真实程序的空间局部性通常在**较小的范围**内是有效的，例如几条指令或少量数据单元。
   - 如果块过大，则块内部的数据彼此之间的访问可能性降低，使得大块数据的有效性减弱。

### 3. **结论**
当块过大时，虽然仍然能利用一定的空间局部性，但由于：
- 访问模式导致有效数据减少，
- 缓存块数减少导致替换频繁，
- 过大的数据范围导致空间局部性效果下降，

最终，块的增大会减少缓存的有效性，使得空间局部性对缓存命中率的提升作用变小，甚至可能导致更高的缓存缺失率（Miss Rate）。
