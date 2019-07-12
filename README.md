## VGG-NST

背景：使用VGG做图像风格迁移

内容：对内容损失和风格损失组合，训练模型输出带有风格特征的内容图像

准备：TensorFlow、numpy、scipy、(vgg.mat)等

Neural Style Transfer 神经风格迁移，是计算机视觉流行的一种算法，最先论文来源《 A Neural Algorithm of Artistic Style》

所谓的图像风格迁移，是指利用算法学系著名画作的风格，将这个风格应用到我们自定义的图片上，其中著名的图像处理应用Prisma是用利用风格迁移技术，将普通人的照片自动转换为具有艺术气息风格的图片。

将用到ImageNet VGG模型来图像风格迁移，其实VGGNet的本意并不是为了风格迁移，而是输入图像，提取图像中的特征，输出图像的类别，即图像识别，而图像风格迁移恰好与其相反，输入的是特征，输出的具有这种特征的图片。

风格迁移使用卷积层的中间特征还原对应的特征原始图像，个人理解：你使用一系列的卷积层对底层图（称content_img）进行特征学习，现在你又将某一层/几层的特征返过来试着生成原始图，显然你没有使用全部卷积层是几乎不能生成与原图完全一样的图形（可以理解为“反卷积”操作），那么你生成的图（称Generated_img）与原图就存在差异，这种差异可以称为内容损失（content loss），另外浅层卷积层还原效果会比深层卷积层好（深层卷积层往往只保留图像中物体形状和位置），另外是使用梯度下降法还原图像，以内容损失作为损失/优化函数。

除了使用图像还原，还需要对内容加入“风格”，这里就要引入我们的风格图(style_img)，可以使用风格图的卷积层特征的Gram矩阵去表示“风格”，Gram矩阵即格拉姆矩阵，是关于一组向量的内积的对称矩阵，是在n维欧式空间中任意k(k≤n)个向量α1,α2,⋯,αk的内积所组成的矩阵，这里就不过多描述。

总体而言，实现图像风格迁移即将内容损失(内容/底层图与内容还原图)和风格损失(风格图与风格还原图)的组合，是由两个神经网络组合而成，利用内容损坏和风格损失训练图像生成网络。

NST算法步骤大致总结一下：

①构建content图像损失函数loss(C,Gc)

②构建style图像损失函数loss(S,Gs)

③生成合并：rs_img = αloss(C,Gc)+βloss(S,Gs)

效果图：

<img src="https://github.com/jm199504/VGG-NST/blob/master/images/1.png">

