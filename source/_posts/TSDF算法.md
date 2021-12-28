---
title: TSDF算法
date: 2021-12-23 16:54:25
tags: SLAM
---

具体学习于 [https://github.com/andyzeng/tsdf-fusion](https://github.com/andyzeng/tsdf-fusion)
### Why TSDF
三维重建首当其冲的问题是如何保存以及如何表示模型，通常而言会有点云，mesh等；
但是对于室内三维重建，TSDF是一个不错的选择，其优点是
- 非常适合CUDA并行运算，从而达到实时。
- 开辟固定的内存/显存，模型大小相对可控
- 模型大小不随数据量变化，网格的细节比较好
缺点：CPU计算耗时，在边缘以及前后景交界出现拖尾现象（体素g在像素坐标系投影有一定的误差）

### How TSDF

1.建立长方体包围盒（能包住房间，一般预设参数：可通过设定划分网格以及网格大小得到)
2.将每个体素v转化成三维座标点g(根据模型起点和网格推算)
3.对于新来的每一帧深度图：
    遍历每一个体素g：
        3.1.根据相机外参，将g在世界坐标系转换到相机坐标系的点c,再由相机内参转换到像素坐标系x;
        3.2.深度相机像素深度为value(x),点c到相机坐标原点的距离为distance(v)
        3.3.sdf(g) = value(x) - distance(v).
        3.4.求tsdf(g).`预设截断距离t = voxel_size * t_n, 则t以内，tsdf(g) = sdf(p)/|u|;在t以外时，if sdf(p)>1, tsdf(p) = 1;if sdf(p)<-1, tsdf(p)=-1;`
        3.5.权重w(p) = cos(theta)/distance(v), theta为投影光线与表面法向的夹角
至此，得到当前帧所有体素的tsdf值以及权重值
4.当前帧与全局模型融合
    4.1 若当前帧为第一帧，即为融合结果；否则与之前的模型融合,公式如下，其中W(p)为融合权重，w(p)为当前帧权重
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xMTEzMzY5Ni02ZWY1Yzk2YWUwMWFlNTZmLnBuZw?x-oss-process=image/format,png)
    
### Detail and Analysis
