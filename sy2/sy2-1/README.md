# 创建第一个Android Kotlin应用

### 1. 创建一个新的工程

- #### 思路

  ① 打开Android Studio，选择Projects>New Project，然后选择Basic Activity.

  ② 点击Next，为应用程序命名，选择Kotlin语言，然后点击Finish。Android Studio将使用系统中最新的API Level创建应用程序，并使用Gradle作为构建系统，在底部的视窗中可以查看整个过程。
  
  ③ 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.1.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.2.png" alt="avatar" style="zoom:50%; width:750px" />


### 2. 探索Android Studio的界面布局
- #### 思路

  ① 整个Android Studio工作区包括多个部分

  ② 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.3.png" alt="avatar" style="zoom:50%; width:750px" />


### 3. 创建模拟器

- #### 思路

  ① 此步骤创建可以运行APP的模拟器，点击Tool>Device Manager或者工具栏上的按钮

  ② 点击Create device，弹出创建模拟器的页面
  
  ③ 选择想要创建模拟器设备（如Pixel 5），点击Next，在系统镜像页面的Recommended标签栏，选择最新镜像
  
  ④ 然后首先下载镜像（Download），下载完成之后点击Next，完成模拟器命名和更多参数选择，最终点击Finish完成
  
  ⑤ 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.4.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.5.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.6.png" alt="avatar" style="zoom:50%; width:750px" />
  
  ### 4.在模拟器上运行应用程序

- #### 思路

  ① 选择Run>Run ‘app’，在工具栏上可以看到运行程序的一些选择项
  
  ② 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.7.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.8.png" alt="avatar" style="zoom:50%; width:750px" />
  
  ### 5.查看布局编辑器

- #### 思路

  ①在Basic Activity中，包含了基本的导航组件，Android app关联两个fragments，第一个屏幕显示了“Hello first fragment”由FirstFragment创建，界面元素的排列由布局文件指定，查看res>layout>fragment_first.xml

  ②（1）查看布局的代码（Code），修改Textview的Text属性
  
  （2）实验代码：
  
  ```xml
  android:text="@string/hello_first_fragment"
  ```
  ③（1）右键该代码，选择Go To > Declaration or Usages，跳转到values/strings.xml，看到高亮文本
  
  （2）实验代码：
  ```java
  <string name="hello_first_fragment">Hello first fragment</string>
  ```
  ④ 修改字符串属性值为“Hello Kotlin!”。更进一步，修改字体显示属性，在Design视图中选择textview_first文本组件，在Common Attributes属性下的textAppearance域，设置相关的文字显示属性
  
  ⑤查看布局的XML代码，可以看到新属性被应用
 
  ```xml
  android:fontFamily="sans-serif-condensed"
  android:text="@string/hello_first_fragment"
  android:textColor="@android:color/darker_gray"
  android:textSize="30sp"
  android:textStyle="bold"
  ```
  ⑤重新运行应用程序，实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.9.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.10.png" alt="avatar" style="zoom:50%; width:750px" />

### 6.查看视图的布局约束

- #### 思路

  ① 在fragment_first.xml，查看TextView组件的约束属性

  ② 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.11.png" alt="avatar" style="zoom:50%; width:750px" />

### 7.添加按钮和约束

- #### 思路

  ① 从Palette面板中拖动Button到
  
  ② 调整Button的约束，设置Button的Top>BottonOf textView
  ```xml
      app:layout_constraintTop_toBottomOf="@+id/textview_first" />
  ```
  
  ③ 随后添加Button的左侧约束至屏幕的左侧，Button的底部约束至屏幕的底部。查看Attributes面板，修改将id从button修改为toast_button
  
  ④ 实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.12.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 8.调整Next按钮

- #### 思路

   ① Next按钮是工程创建时默认的按钮，查看Next按钮的布局设计视图，它与TextView之间的连接不是锯齿状的而是波浪状的，表明两者之间存在链（chain），是一种两个组件之间的双向联系而不是单向联系。删除两者之间的链，可以在设计视图右键相应约束，选择Delete（注意两个组件要双向删除）

   ② 删除Next按钮的左侧约束
  
   ③ 实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.13.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 9.添加新的约束

- #### 思路

   ① 添加Next的右边和底部约束至父类屏幕（如果不存在的话），Next的Top约束至TextView的底部。最后，TextView的底部约束至屏幕的底部

   ② 实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.14.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 10.更改组件的文本

- #### 思路

   ① fragment_first.xml布局文件代码中，找到toast_button按钮的text属性部分
   ```xml
   <Button
     android:id="@+id/toast_button"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:text="Button"
  ```

   ② 这里text的赋值是一种硬编码，点击文本，左侧出现灯泡状的提示，选择 Extract string resource
  
   ③ 弹出对话框，令资源名为toast_button_text，资源值为Toast，并点击OK
   
   ④ 实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.15.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.16.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 11.更新Next按钮

- #### 思路

   ① 在属性面板中更改Next按钮的id，从button_first改为random_button
   ```xml
   <Button
     android:id="@+id/toast_button"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:text="Button"
  ```

   ② 在string.xml文件，右键next字符串资源，选择 Refactor > Rename，修改资源名称为random_button_text，点击Refactor 。随后，修改Next值为Random
  
   ③ 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.17.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 12.添加第三个按钮

- #### 思路

   ① 向fragment_first.xml文件中添加第三个按钮，位于Toast和Random按钮之间，TextView的下方。新Button的左右约束分别约束至Toast和Random，Top约束至TextView的底部，Buttom约束至屏幕的底部

   ② 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.18.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 13.完善UI组件的属性设置

- #### 思路

   ① 更改新增按钮id为count_button，显示字符串为Count，对应字符串资源值为count_button_text

   ② 同时，更改TextView的文本为0。修改后的fragment_first.xml的代码
  ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstFragment">

    <TextView
        android:id="@+id/textview_first"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-condensed"
        android:text="@string/hello_first_fragment"
        android:textColor="@android:color/darker_gray"
        android:textSize="30sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/random_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/random_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textview_first" />

    <Button
        android:id="@+id/toast_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/toast_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textview_first" />

    <Button
        android:id="@+id/count_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/count_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/random_button"
        app:layout_constraintStart_toEndOf="@+id/toast_button"
        app:layout_constraintTop_toBottomOf="@+id/textview_first" />
   </androidx.constraintlayout.widget.ConstraintLayout>

  ```
   
   ### 14.更新按钮和文本框的外观

- #### 思路

   ① 添加新的颜色资源：values>colors.xml定义了一些应用程序可以使用的颜色，添加新颜色screenBackground 值为 #2196F3，这是蓝色阴影色；添加新颜色buttonBackground 值为 #BBDEFB
   
   ```xml
   <color name="screenBackground">#2196F3</color>
   <color name="buttonBackground">#BBDEFB</color>
   ```
  
   ② 设置组件的外观：fragment_first.xml的属性面板中设置屏幕背景色为
   
   ```xml
   android:background="@color/screenBackground"
   ```
   
   设置每个按钮的背景色为buttonBackground
  
   ```xml
   android:background="@color/buttonBackground"
   ```
   
   移除TextView的背景颜色，设置TextView的文本颜色为color/white，并增大字体大小至72sp
   
   ### 15.设置组件的位置

- #### 思路

   ① Toast与屏幕的左边距设置为24dp，Random与屏幕的右边距设置为24dp，利用属性面板的Constraint Widget完成设置

   ② 设置TextView的垂直偏移为0.3
   ```xml
   app:layout_constraintVertical_bias="0.3"
   ```
  
   ③ 拖动左侧的移动条。
   
   ④ 运行程序。
   
   ⑤ 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.19.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.20.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.21.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 16.设置代码自动补全

- #### 思路

   ① Android Studio中，依次点击File>New Projects Settings>Settings for New Projects…，查找Auto Import选项，在Java和Kotlin部分，勾选Add Unambiguous Imports on the fly

   ② 实验结果如下图：
   
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.22.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 17.TOAST按钮添加一个toast消息

- #### 思路

   ① 打开FirstFragment.kt文件，有三个方法：onCreateView，onViewCreated和onDestroyView，在onViewCreated方法中使用绑定机制设置按钮的响应事件（创建应用程序时自带的按钮）
   ```java
   binding.randomButton.setOnClickListener {
    findNavController().navigate(R.id.action_FirstFragment_to_SecondFragment)
   }
   ```

   ② 接下来，为TOAST按钮添加事件，使用findViewById()查找按钮id，代码如下
   ```java
   // find the toast_button by its ID and set a click listener
   view.findViewById<Button>(R.id.toast_button).setOnClickListener {
     // create a Toast with some text, to appear for a short time
     val myToast = Toast.makeText(context, "Hello Toast!", Toast.LENGTH_LONG)
     // show the Toast
     myToast.show()
   } 

   ```
   
   ### 18.使Count按钮更新屏幕的数字

- #### 思路

   ① 在FirstFragment.kt文件，为count_buttion按钮添加事件
   ```java
   view.findViewById<Button>(R.id.count_button).setOnClickListener {
     countMe(view)
   }
   ```

   ② countMe()为自定义方法，以View为参数，每次点击增加数字1，具体代码为
   ```java
   private fun countMe(view: View) {
     // Get the text view
     val showCountTextView = view.findViewById<TextView>(R.id.textview_first)

     // Get the value of the text view.
     val countString = showCountTextView.text.toString()

     // Convert value to a number and increment it
     var count = countString.toInt()
     count++

     // Display the new value in the text view.
     showCountTextView.text = count.toString()
   }
   ```
   
   ### 19.完成第二界面的代码

- #### 思路

   ① 此步骤将完成按照First Fragment显示数字作为上限，随机在Second Fragment上显示一个数字，即Random按钮的事件响应

   ### 20.向界面添加TextView显示随机数

- #### 思路

   ① 打开fragment_second.xml的设计视图中，当前界面有两个组件，一个Button和一个TextView（textview_second）

   ② 去掉TextView和Button之间的约束
   
   ③ 拖动新的TextView至屏幕的中间位置，用来显示随机数
   
   ④ 设置新的TextView的id为**@+id/textview_random**
   
   ⑤ 设置新的TextView的左右约束至屏幕的左右侧，Top约束至textview_second的Bottom，Bottom约束至Button的Top
   
   ⑥ 设置TextView的字体颜色textColor属性为**@android:color/white**，textSize为72sp，textStyle为bold
   
   ⑦ 设置TextView的显示文字为“R”
   
   ⑧ 设置垂直偏移量layout_constraintVertical_bias为0.45
   
   ⑨新增TextView最终的属性代码
   ```xml
   <TextView
   android:id="@+id/textview_random"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="R"
   android:textColor="@android:color/white"
   android:textSize="72sp"
   android:textStyle="bold"
   app:layout_constraintBottom_toTopOf="@+id/button_second"
   app:layout_constraintEnd_toEndOf="parent"
   app:layout_constraintStart_toStartOf="parent"
   app:layout_constraintTop_toBottomOf="@+id/textview_second"
   app:layout_constraintVertical_bias="0.45" />
   }
   ```
   
   ### 21.更新显示界面文本的TextView(textview_second)

- #### 思路

   ① 在fragment_second.xml文件中，选择textview_second文本框，查看text属性，可见
   ```xml
   android:text="@string/hello_second_fragment
   ```
   对应的strings.xml文本为Hello second fragment. Arg: %1$s

   ② 更改该文本框id为textview_header
   
   ③ 设置layout_width为match_parent，layout_height为wrap_content
   
   ④ 设置top，left和right的margin为24dp，左边距和右边距也就是start和end边距
   
   ⑤ 若还存在与Button的约束，则删除
   
   ⑥ 向colors.xml添加颜色colorPrimaryDark，并将TextView颜色设置为@color/colorPrimaryDark，字体大小为24sp
   ```xml
   <color name="colorPrimaryDark">#3700B3</color>
   ```
   
   ⑦ strings.xml文件中，修改hello_second_fragment的值为"Here is a random number between 0 and %d."
   
   ⑧ 使用Refactor>Rename将hello_second_fragment 重构为random_heading
   
   ⑨ 因此，显示界面信息的Textview的代码为
   ```xml
   <TextView
   android:id="@+id/textview_header"
   android:layout_width="0dp"
   android:layout_height="wrap_content"
   android:layout_marginStart="24dp"
   android:layout_marginLeft="24dp"
   android:layout_marginTop="24dp"
   android:layout_marginEnd="24dp"
   android:layout_marginRight="24dp"
   android:text="@string/random_heading"
   android:textColor="@color/colorPrimaryDark"
   android:textSize="24sp"
   app:layout_constraintEnd_toEndOf="parent"
   app:layout_constraintStart_toStartOf="parent"
   app:layout_constraintTop_toTopOf="parent" />
   ```
   
   ### 22.TOAST按钮添加一个toast消息

- #### 思路

   ① 向colors.xml文件添加第二个Fragment背景色的值，修改fragment_second.xml背景色的属性为screenBackground2
   ```xml
   <color name="screenBackground2">#26C6DA</color>
   ```

   ② 将按钮移动至界面的底部
   
   ### 23.检查导航图

- #### 思路

   ① 本项目选择Android的Basic Activity类型进行创建，默认情况下自带两个Fragments，并使用Android的导航机制Navigation。导航将使用按钮在两个Fragment之间进行跳转，就第一个Fragment修改后的Random按钮和第二个Fragment的Previous按钮，打开nav_graph.xml文件（res>navigation>nav_graph.xml），可以任意拖动界面中的元素，观察导航图的变化。

   ② 实验结果如下图：
   
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.23.png" alt="avatar" style="zoom:50%; width:750px" />

   ### 24.启用SafeArgs

- #### 思路

   ① 首先打开 Gradle Scripts > build.gradle (Project: My First App)
 
   ② 找到buildscript脚本中的dependencies章节，添加如下代码
   ```
   def nav_version = "2.3.0-alpha02"
   classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
   ```
   
   ③ 接着打开 Gradle Scripts > build.gradle (Module: app)
   
   ④ apply plugin开头的代码下添加一行
   ```
   apply plugin: 'androidx.navigation.safeargs.kotlin'
   ```
   ⑤ Android Studio开始同步依赖库
   
   ⑥ 重新生成工程Build > Make Project
 
   ### 25.创建导航动作的参数

- #### 思路

   ① 打开导航视图，点击FirstFragment，查看其属性
 
   ② 在Actions栏中可以看到导航至SecondFragment
   
   ③ 同理，查看SecondFragment的属性栏
   
   ④ 点击Arguments +符号
   
   ⑤ 弹出的对话框中，添加参数myArg，类型为整型Integer
  
   ### 26.FirstFragment添加代码，向SecondFragment发数据

- #### 思路

   ① 打开FirstFragment.kt源代码文件
 
   ② 找到onViewCreated()方法，该方法在onCreateView方法之后被调用，可以实现组件的初始化。找到Random按钮的响应代码，注释掉原先的事件处理代码
   
   ③ 实例化TextView，获取TextView中文本并转换为整数值
   ```
   val showCountTextView = view.findViewById<TextView>(R.id.textview_first)
   val currentCount = showCountTextView.text.toString().toInt()
   ```
   
   ④ 将currentCount作为参数传递给actionFirstFragmentToSecondFragment()
   ```
   val action = FirstFragmentDirections.actionFirstFragmentToSecondFragment(currentCount)
   ```
   
   ⑤ 添加导航事件代码
   ```
   findNavController().navigate(action)
   ```
   
   ⑥运行代码，点击FirstFragment的Count按钮，然后点击Random按钮，可以看到SecondFragment在头部的TextView已经显示正确的数字，但是屏幕中间还未出现随机数显示
   
   ### 27.添加SecondFragment的代码

- #### 思路

   ① 导入navArgs包
   ```
   import androidx.navigation.fragment.navArgs
   ```
 
   ② onViewCreated()代码之前添加一行
   ```
   val args: SecondFragmentArgs by navArgs()
   ```
   
   ③ onViewCreated()中获取传递过来的参数列表，提取count数值，并在textview_header中显示
   ```
   val count = args.myArg
   val countText = getString(R.string.random_heading, count)
   view.findViewById<TextView>(R.id.textview_header).text = countText
   ```
   
   ④ 根据count值生成随机数
   ```
   val random = java.util.Random()
   var randomNumber = 0
   if (count > 0) {
      randomNumber = random.nextInt(count + 1)
   }
   ```
   
   ⑤ textview_random中显示count值
   ```
   view.findViewById<TextView>(R.id.textview_random).text = randomNumber.toString()
   ```
   
   ⑥运行应用程序，查看运行结果，最终结果如下图所示：
   
- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.24.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.1.25.png" alt="avatar" style="zoom:50%; width:750px" />
