# 基于TensorFlow Lite实现的Android花卉识别应用

### 1. 下载初始代码

（1） 创建工作目录，使用

```
git clone https://github.com/hoitab/TFLClassify.git
```

拷贝代码；或者直接访问github链接下载代码的ZIP包，并解压缩到工作目录。

### 2. 运行初始代码

（1）打开Android Studio，选择“Open an Existing Project”

（2）选择TFLClassify/build.gradle生成整个项目。项目包含两个module：finish 和 start，finish模块是已经完成的项目，start则是本项目实践的模块。

（3）第一次编译项目时，弹出“Gradle Sync”，将下载相应的gradle wrapper 。

（4）手机通过USB接口连接开发平台，并设置手机开发者选项允许调试。

（5）选择真实物理机（而不是模拟器）运行start模块

- <img src="https://github.com/17515424731/Project/blob/main/image/4.6.png" alt="avatar" style="zoom:50%; width:750px" />

（6）允许应用获取手机摄像头的权限，得到下述效果图，界面利用随机数表示虚拟的识别结果。

- <img src="https://github.com/17515424731/Project/blob/main/image/4.7.jpg" alt="avatar" style="zoom:50%; width:750px" />
### 3.向应用中添加TensorFlow Lite

（1）选择"start"模块

（2）右键“start”模块，或者选择File，然后New>Other>TensorFlow Lite Model

- <img src="https://github.com/17515424731/Project/blob/main/image/4.1.png" alt="avatar" style="zoom:50%; width:750px" />

（3）选择已经下载的自定义的训练模型。本教程模型训练任务以后完成，这里选择finish模块中ml文件下的FlowerModel.tflite。

- <img src="https://github.com/17515424731/Project/blob/main/image/4.2.png" alt="avatar" style="zoom:50%; width:750px" />

点击“Finish”完成模型导入，系统将自动下载模型的依赖包并将依赖项添加至模块的build.gradle文件。

（4）最终TensorFlow Lite模型被成功导入，并生成摘要信息

- <img src="https://github.com/17515424731/Project/blob/main/image/4.3.png" alt="avatar" style="zoom:50%; width:750px" />

### 4.检查代码中的TODO项

（1）本项目初始代码中包括了若干的TODO项，以导航项目中未完成之处。为了方便起见，首先查看TODO列表视图，View>Tool Windows>TODO

- <img src="https://github.com/17515424731/Project/blob/main/image/4.4.png" alt="avatar" style="zoom:50%; width:750px" />

（2）默认情况下了列出项目所有的TODO项，进一步按照模块分组（Group By）

- <img src="https://github.com/17515424731/Project/blob/main/image/4.5.png" alt="avatar" style="zoom:50%; width:750px" />

### 5.添加代码重新运行APP

（1）定位“start”模块MainActivity.kt文件的TODO 1，添加初始化训练模型的代码

```
private class ImageAnalyzer(ctx: Context, private val listener: RecognitionListener) :
        ImageAnalysis.Analyzer {

  ...
  // TODO 1: Add class variable TensorFlow Lite Model
  private val flowerModel = FlowerModel.newInstance(ctx)

  ...
}
```

（2）在CameraX的analyze方法内部，需要将摄像头的输入ImageProxy转化为Bitmap对象，并进一步转化为TensorImage 对象

```
override fun analyze(imageProxy: ImageProxy) {
  ...
  // TODO 2: Convert Image to Bitmap then to TensorImage
  val tfImage = TensorImage.fromBitmap(toBitmap(imageProxy))
  ...
}
```

（3）对图像进行处理并生成结果，主要包含下述操作：

按照属性score对识别结果按照概率从高到低排序

列出最高k种可能的结果，k的结果由常量MAX_RESULT_DISPLAY定义

```
override fun analyze(imageProxy: ImageProxy) {
  ...
  // TODO 3: Process the image using the trained model, sort and pick out the top results
  val outputs = flowerModel.process(tfImage)
      .probabilityAsCategoryList.apply {
          sortByDescending { it.score } // Sort with highest confidence first
      }.take(MAX_RESULT_DISPLAY) // take the top results

  ...
}
```

（4）将识别的结果加入数据对象Recognition 中，包含label和score两个元素。后续将用于RecyclerView的数据显示

```
override fun analyze(imageProxy: ImageProxy) {
  ...
  // TODO 4: Converting the top probability items into a list of recognitions
  for (output in outputs) {
      items.add(Recognition(output.label, output.score))
  }
  ...
}
```

（5）将原先用于虚拟显示识别结果的代码注释掉或者删除

```
// START - Placeholder code at the start of the codelab. Comment this block of code out.
for (i in 0..MAX_RESULT_DISPLAY-1){
    items.add(Recognition("Fake label $i", Random.nextFloat()))
}
// END - Placeholder code at the start of the codelab. Comment this block of code out.
```

（6）以物理设备重新运行start模块

（7）最终运行效果

- <img src="https://github.com/17515424731/Project/blob/main/image/4.8.jpg" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/4.9.jpg" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/4.10.jpg" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/4.11.jpg" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/4.12.jpg" alt="avatar" style="zoom:50%; width:750px" />
