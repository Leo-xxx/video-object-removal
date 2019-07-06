## 视频编辑利器，不喜欢就框除！开源视频物体移除软件video object removal

原创： CV君 [我爱计算机视觉](javascript:void(0);) *前天*

点击我爱计算机视觉标星，更快获取CVML新技术

------



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTsQxwvxOG8oVYllUjZ3iatsNJOf90Y17rX8YGbWXztr4h1Ldvgh2qeLsCzMZI37bTTAK8y0vwAQqEA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

image inpainting



像上图中的image inpainting相信大家并不陌生，OpenCV就有相关的例子。



但如果是去除视频里的目标呢，就不那么容易了。



今天跟大家介绍一款基于PyTorch的video object removal的开源库，只用在第一帧框出物体就可以将框出的物体在视频中移除，操作很方便，移除的效果也不错。



代码链接：

https://github.com/zllrunning/video-object-removal



对于移除图像中的物体，我们可以先将物体使用mask遮住，再用image inpainting来解决，而视频中如果想移除某个物体呢？



可以简单的使用拆帧按照图像来处理，但是视频中的物体会有移动，那我们需要每幅图像的mask，这会带来很大的工作量，而且这种思路没有考虑到视频中时序上的关系。



CVPR 2019有篇文章Deep Video Inpainting[1]介绍了一种视频的处理方法，生成效果时序上更加一致，但此方法同样需要视频中每帧的物体的mask，如果想使用此方法那我们就需要想办法获取物体的mask。



在3月7日，52CV发了关于SiamMask[2]目标跟踪算法的文章（[CVPR 2019 | 惊艳的SiamMask：开源快速同时进行目标跟踪与分割算法](http://mp.weixin.qq.com/s?__biz=MzIwMTE1NjQxMQ==&mid=2247485846&idx=1&sn=daac68cec67ed28114a61e94d7cceaa4&chksm=96f37bc2a184f2d437b3129ded8057a4c664d8069b5f462ea15141f10ebde94c551ce0f9dd6e&scene=21#wechat_redirect)），将跟踪与分割结合起来，能够对给出目标的像素级标注。



那SiamMask提供视频帧中物体的mask，Deep Video Inpainting负责物体的移除、修复，便可以实现一键移除视频中的物体，解放了标注mask的双手。



这就是本文介绍的这款开源软件的做法。



软件使用也非常简单。



在配置好环境后，只需要一条命令：

- 

```
python demo.py --data data/Human6
```



支持视频文件的测试：

- 

```
python demo.py --data data/bag.avi
```



如果你发现这个物体的边缘处理的不是很干净你可以更改--mask-dilation这个参数，作用是扩大遮罩的范围：

- 

```
python demo.py --data data/Human6  --mask-dilation 32
```



最后看下效果：



![img](https://mmbiz.qpic.cn/mmbiz_gif/BJbRvwibeSTsQxwvxOG8oVYllUjZ3iatsN89c0amgNdPnFKpfDqyGoLGicdpuGIN13sfPSQ6KusUbbW2hjJasNS9A/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

操作示例，框出要移除的物体



修复后的视频会自动存储results/inpaint文件夹下。



![img](https://mmbiz.qpic.cn/mmbiz_gif/BJbRvwibeSTsQxwvxOG8oVYllUjZ3iatsNldwgoKZXjLADILVfHraiasujyHFQs6q0nxYMq5ibhJo03tg1R60yVQ5g/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



感觉用这个可以做很多有意思的应用，你有什么想法吗？欢迎留言。



参考文献

[1] Wang Q, Zhang L, Bertinetto L, et al. Fast online object tracking and segmentation: A unifying approach[C]//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2019: 1328-1338.

[2] Kim, Dahun and Woo, Sanghyun and Lee, Joon-Young and So Kweon, In, Deep Video Inpainting//Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition.2019: 5792—5801.



这份代码刚刚开源，感谢开发者，也欢迎大家给大佬加星。

https://github.com/zllrunning/video-object-removal



相关阅读：

[利用OpenCV抠图技术实现影视隐身特效](http://mp.weixin.qq.com/s?__biz=MzUzODkxNzQzMw==&mid=2247483718&idx=1&sn=f30f8ff180413e936b6945bec84cda83&chksm=fad12e10cda6a706951b2b57e89ecec0a25500e994cb8e9d14709a2e50c4e2f75e8252633a19&scene=21#wechat_redirect)

[CVPR 2019 论文大盘点-目标跟踪篇](http://mp.weixin.qq.com/s?__biz=MzIwMTE1NjQxMQ==&mid=2247487537&idx=2&sn=8fe8b7cc85f5da00032e7a85fc13f864&chksm=96f36265a184eb73b9183ad59206045b221125fb05fed566eac51be791f06f85f8c5666d0f12&scene=21#wechat_redirect)



------



**CV细分方向交流群**



关注最新最前沿的计算机视觉技术，欢迎加入52CV细分方向交流群，扫码添加CV君拉你入群（如已为CV君好友，请直接私信，**不必重复添加**），

**（请务必注明关注方向，比如：跟踪）：**

**![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTs1Ke4WXicIqN7QibMXL527MCvicgajlnePVw1mnomoLqFqL0WLf7UUpSkVGj2E1GGe83e8ZmY0G42jw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**

喜欢在QQ交流的童鞋可以加52CV官方QQ群：702781905。

（不会时时在线，如果没能及时通过还请见谅）

------

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTvVOnJBvePcP1qFUSWpyvrjpYAWNIZTZzUA7Zq4VPlReicJWcIeozxic5VhHlwNQNAFXmKQBtKf5xAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

长按关注我爱计算机视觉







![img](https://mp.weixin.qq.com/mp/qrcode?scene=10000004&size=102&__biz=MzIwMTE1NjQxMQ==&mid=2247487549&idx=2&sn=7e3c2edfb464782b6345d9660b231593&send_time=)

微信扫一扫
关注该公众号