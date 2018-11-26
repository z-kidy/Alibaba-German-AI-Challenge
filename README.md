# Alibaba-German-AI-Challenge
多通道城市图像分类
### 2018.11.15
* 第一次线上测评，利用两层卷积两层全连接网络制作的baseline线上accuracy达到0.702，当天排名第二名
  * 该baseline使用的时GD算法进行参数寻优，之后可以使用Adam算法进行替代观察线上成绩
* 根据官方给出的analysis文件中EDA的方法，plot出一个样本的18个通道的图像
* 由于在dataset中label是one-hot形式的，所以利用一下代码计算出vali数据集中各类别的数量:
<pre><code>label_qty = np.sum(label, axis=0)</code></pre>
### 2018.11.16
* 第二次线上测评，网络结构并不发生变化，使用adam算法替换gd算法，提高收敛速度
* 线上accuracy是0.663，当天排名第一，总排名依然第二
* 本次实验使用10k个batch进行训练，batch_size是200
### 2018.11.25
* 网络结构采用两个3 * 3卷积层+池化+两个3 * 3卷积层+池化+3层全连接(1024+2048+17)
* 优化算法对比了Adam、RSMProp、SGD，得出结论，RSMProp不适合mini-batch学习，使用该方法学习率会忽上忽下变化极大
* SGD算法固定学习率导致收敛速度非常慢，做高效率实验还是要使用Adam算法寻优
* 在固定网络结构，固定优化算法以及学习率的情况下，改变batch_size作对比实验
  * 优化算法：RSMProp，batch_size:256，learning_rate:1e-4时，线下vali_acc收敛于0.75左右，线上acc0.686
  * 优化算法：Adam，batch_size:1024,learning_rate:1e-4时，线下vali_acc收敛于0.76左右，线上acc0.724，总排名14
* 接下来固定使用Adam进行对比实验，优化网络结构以及数据输入，做更大的batch_size的对比试验，尝试增加全连接层数量以及节点数观察分类效果
* 尝试制作两个分类器（s1，s2）分别只使用两个卫星的数据进行训练，比较两个分类器性能
