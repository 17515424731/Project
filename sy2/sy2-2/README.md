# Android CameraX的基础使用

### 1. 创建项目

- #### 思路

  ① 首先创建一个新项目，选择“Empty Activity”。

  ② 将项目命名为“CameraXApp”，软件包名称更改为“com.android.example.cameraxapp”。选择Kotlin语言开发，设定最低支持的API Level 21（CameraX 所需的最低级别）
  
  ③ 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.1.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.2.png" alt="avatar" style="zoom:50%; width:750px" />


### 2. 添加 Gradle 依赖
- #### 思路

  ① 打开项目的模块（Module）的build.gradle 文件，并添加 CameraX 依赖项
```
dependencies {
  def camerax_version = "1.1.0-beta01"
  implementation "androidx.camera:camera-core:${camerax_version}"
  implementation "androidx.camera:camera-camera2:${camerax_version}"
  implementation "androidx.camera:camera-lifecycle:${camerax_version}"
  implementation "androidx.camera:camera-video:${camerax_version}"

  implementation "androidx.camera:camera-view:${camerax_version}"
  implementation "androidx.camera:camera-extensions:${camerax_version}"
}
```
  ② CameraX 需要一些属于 Java 8 的方法，因此需要相应地设置编译选项（实际上比较新的Android Studio版本会默认设置）。在 android 代码块的末尾，紧跟在 buildTypes 之后，添加以下代码
  ```
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  }
  ```
  ③ CameraX 需要一些属于 Java 8 的方法，因此需要相应地设置编译选项（实际上比较新的Android Studio版本会默认设置）。在 android 代码块的末尾，紧跟在 buildTypes 之后，添加以下代码
```
compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
}

```
  ④ 因为在项目中使用了ViewBinding，在 android{} 代码块末尾添加如下代码
  ```
  buildFeatures {
     viewBinding true
  }
  ```
  
  ⑤ 左侧出现提示时（灯泡状），点击 Sync Now，于是便可以在应用中使用 CameraX 了

### 3.创建项目布局

- #### 思路

  ① 打开res/layout/activity_main.xml 的 activity_main 布局文件，并将其替换为以下代码
 
  ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <androidx.constraintlayout.widget.ConstraintLayout
   xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MainActivity">

   <androidx.camera.view.PreviewView
       android:id="@+id/viewFinder"
       android:layout_width="match_parent"
       android:layout_height="match_parent" />

   <Button
       android:id="@+id/image_capture_button"
       android:layout_width="110dp"
       android:layout_height="110dp"
       android:layout_marginBottom="50dp"
       android:layout_marginEnd="50dp"
       android:elevation="2dp"
       android:text="@string/take_photo"
       app:layout_constraintBottom_toBottomOf="parent"
       app:layout_constraintLeft_toLeftOf="parent"
       app:layout_constraintEnd_toStartOf="@id/vertical_centerline" />

   <Button
       android:id="@+id/video_capture_button"
       android:layout_width="110dp"
       android:layout_height="110dp"
       android:layout_marginBottom="50dp"
       android:layout_marginStart="50dp"
       android:elevation="2dp"
       android:text="@string/start_capture"
       app:layout_constraintBottom_toBottomOf="parent"
       app:layout_constraintStart_toEndOf="@id/vertical_centerline" />

   <androidx.constraintlayout.widget.Guideline
       android:id="@+id/vertical_centerline"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:orientation="vertical"
       app:layout_constraintGuide_percent=".50" />

  </androidx.constraintlayout.widget.ConstraintLayout>

  ```
  
  ② 更新res/values/strings.xml 文件 
  
 ```xml

   <resources>
   <string name="app_name">CameraXApp</string>
   <string name="take_photo">Take Photo</string>
   <string name="start_capture">Start Capture</string>
   <string name="stop_capture">Stop Capture</string>
   </resources>

 ```
  
  ③ 实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.3.png" alt="avatar" style="zoom:50%; width:750px" />
  
  ### 4.编写 MainActivity.kt 代码

- #### 思路

  ①将 MainActivity.kt 中的代码替换为以下代码，但保留软件包名称不变。它包含 import 语句、将要实例化的变量、要实现的函数以及常量。系统已实现 onCreate()，供我们检查相机权限、启动相机、为照片和拍摄按钮设置 onClickListener()，以及实现 cameraExecutor。虽然系统已经实现 onCreate()，但在实现文件中的方法之前，相机将无法正常工作。

```
package com.android.example.cameraxapp

import android.Manifest
import android.content.ContentValues
import android.content.pm.PackageManager
import android.os.Build
import android.os.Bundle
import android.provider.MediaStore
import androidx.appcompat.app.AppCompatActivity
import androidx.camera.core.ImageCapture
import androidx.camera.video.Recorder
import androidx.camera.video.Recording
import androidx.camera.video.VideoCapture
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat
import com.android.example.cameraxapp.databinding.ActivityMainBinding
import java.util.concurrent.ExecutorService
import java.util.concurrent.Executors
import android.widget.Toast
import androidx.camera.lifecycle.ProcessCameraProvider
import androidx.camera.core.Preview
import androidx.camera.core.CameraSelector
import android.util.Log
import androidx.camera.core.ImageAnalysis
import androidx.camera.core.ImageCaptureException
import androidx.camera.core.ImageProxy
import androidx.camera.video.FallbackStrategy
import androidx.camera.video.MediaStoreOutputOptions
import androidx.camera.video.Quality
import androidx.camera.video.QualitySelector
import androidx.camera.video.VideoRecordEvent
import androidx.core.content.PermissionChecker
import java.nio.ByteBuffer
import java.text.SimpleDateFormat
import java.util.Locale

typealias LumaListener = (luma: Double) -> Unit

class MainActivity : AppCompatActivity() {
   private lateinit var viewBinding: ActivityMainBinding

   private var imageCapture: ImageCapture? = null

   private var videoCapture: VideoCapture<Recorder>? = null
   private var recording: Recording? = null

   private lateinit var cameraExecutor: ExecutorService

   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       viewBinding = ActivityMainBinding.inflate(layoutInflater)
       setContentView(viewBinding.root)

       // Request camera permissions
       if (allPermissionsGranted()) {
           startCamera()
       } else {
           ActivityCompat.requestPermissions(
               this, REQUIRED_PERMISSIONS, REQUEST_CODE_PERMISSIONS)
       }

       // Set up the listeners for take photo and video capture buttons
       viewBinding.imageCaptureButton.setOnClickListener { takePhoto() }
       viewBinding.videoCaptureButton.setOnClickListener { captureVideo() }

       cameraExecutor = Executors.newSingleThreadExecutor()
   }

   private fun takePhoto() {}

   private fun captureVideo() {}

   private fun startCamera() {}

   private fun allPermissionsGranted() = REQUIRED_PERMISSIONS.all {
       ContextCompat.checkSelfPermission(
           baseContext, it) == PackageManager.PERMISSION_GRANTED
   }

   override fun onDestroy() {
       super.onDestroy()
       cameraExecutor.shutdown()
   }

   companion object {
       private const val TAG = "CameraXApp"
       private const val FILENAME_FORMAT = "yyyy-MM-dd-HH-mm-ss-SSS"
       private const val REQUEST_CODE_PERMISSIONS = 10
       private val REQUIRED_PERMISSIONS =
           mutableListOf (
               Manifest.permission.CAMERA,
               Manifest.permission.RECORD_AUDIO
           ).apply {
               if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.P) {
                   add(Manifest.permission.WRITE_EXTERNAL_STORAGE)
               }
           }.toTypedArray()
   }
  }

```
  
②（1）在NoteEditor.java中找到onOptionsItemSelected方法，在菜单的switch中添加： 

  （2）实验代码：
  
   ```java
        case R.id.menu_color:
            changeColor();
            break;
   ```
  
### 5.请求必要的权限

- #### 思路
① 应用需要获得用户授权才能打开相机；录制音频也需要麦克风权限；在 Android 9 § 及更低版本上，MediaStore 需要外部存储空间写入权限。在此步骤中，我们将实现这些必要的权限。打开 AndroidManifest.xml，然后将以下代码行添加到 application 标记之前。
  ```xml
  <uses-feature android:name="android.hardware.camera.any" />
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.RECORD_AUDIO" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
     android:maxSdkVersion="28" />
  ```
  
②添加 android.hardware.camera.any 可确保设备配有相机。指定 .any 表示它可以是前置摄像头，也可以是后置摄像头.然后，复制代码到MainActivity.kt. 中
  ```
override fun onRequestPermissionsResult(
   requestCode: Int, permissions: Array<String>, grantResults:
   IntArray) {
   if (requestCode == REQUEST_CODE_PERMISSIONS) {
       if (allPermissionsGranted()) {
           startCamera()
       } else {
           Toast.makeText(this,
               "Permissions not granted by the user.",
               Toast.LENGTH_SHORT).show()
           finish()
       }
   }
}

  ```

  ③运行应用，可发现应用程序请求使用摄像头和麦克风.实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.4.png" alt="avatar" style="zoom:50%; width:750px" />


### 6.实现 Preview 用例

- #### 思路

  ①在相机应用中，取景器用于让用户预览他们拍摄的照片。我们将使用 CameraX Preview 类实现取景器。如需使用 Preview，首先需要定义一个配置，然后系统会使用该配置创建用例的实例。生成的实例就是绑定到 CameraX 生命周期的内容。填充之前的startCamera() 函数

```
  private fun startCamera() {
   val cameraProviderFuture = ProcessCameraProvider.getInstance(this)

   cameraProviderFuture.addListener({
       // Used to bind the lifecycle of cameras to the lifecycle owner
       val cameraProvider: ProcessCameraProvider = cameraProviderFuture.get()

       // Preview
       val preview = Preview.Builder()
          .build()
          .also {
              it.setSurfaceProvider(viewBinding.viewFinder.surfaceProvider)
          }

       // Select back camera as a default
       val cameraSelector = CameraSelector.DEFAULT_BACK_CAMERA

       try {
           // Unbind use cases before rebinding
           cameraProvider.unbindAll()

           // Bind use cases to camera
           cameraProvider.bindToLifecycle(
               this, cameraSelector, preview)

       } catch(exc: Exception) {
           Log.e(TAG, "Use case binding failed", exc)
       }

   }, ContextCompat.getMainExecutor(this))
  }
```

②实验结果如下图：

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.5.png" alt="avatar" style="zoom:50%; width:750px" />

### 7.实现 ImageCapture 用例（拍照功能)

- #### 思路

  ①其他用例与 Preview 非常相似。首先，定义一个配置对象，该对象用于实例化实际用例对象。若要拍摄照片，需要实现 takePhoto() 方法，该方法会在用户按下 photo 按钮时调用。填充takePhoto() 方法的代码：

```
private fun startCamera() {
   val cameraProviderFuture = ProcessCameraProvider.getInstance(this)

   cameraProviderFuture.addListener({
       // Used to bind the lifecycle of cameras to the lifecycle owner
       val cameraProvider: ProcessCameraProvider = cameraProviderFuture.get()

       // Preview
       val preview = Preview.Builder()
           .build()
           .also {
                 it.setSurfaceProvider(viewFinder.surfaceProvider)
           }

       imageCapture = ImageCapture.Builder()
           .build()

       // Select back camera as a default
       val cameraSelector = CameraSelector.DEFAULT_BACK_CAMERA

       try {
           // Unbind use cases before rebinding
           cameraProvider.unbindAll()

           // Bind use cases to camera
           cameraProvider.bindToLifecycle(
               this, cameraSelector, preview, imageCapture)

       } catch(exc: Exception) {
           Log.e(TAG, "Use case binding failed", exc)
       }

   }, ContextCompat.getMainExecutor(this))
}
```
 
  ②重新运行应用，然后按 Take Photo。屏幕上应该会显示一个消息框，会在日志中看到一条消息。这时可以查看本地的图片库，查看刚刚拍摄的图片。实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.6.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.7.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 8.实现 ImageAnalysis 用例

- #### 思路

   ①使用 ImageAnalysis 功能可让相机应用变得更加有趣。它允许定义实现 ImageAnalysis.Analyzer 接口的自定义类，并使用传入的相机帧调用该类。无需管理相机会话状态，甚至无需处理图像；与其他生命周期感知型组件一样，仅绑定到应用所需的生命周期就足够了。将此分析器添加为 MainActivity.kt 中的内部类。分析器会记录图像的平均亮度。如需创建分析器，我们会替换实现 ImageAnalysis.Analyzer 接口的类中的 analyze 函数。

```
private class LuminosityAnalyzer(private val listener: LumaListener) : ImageAnalysis.Analyzer {

   private fun ByteBuffer.toByteArray(): ByteArray {
       rewind()    // Rewind the buffer to zero
       val data = ByteArray(remaining())
       get(data)   // Copy the buffer into a byte array
       return data // Return the byte array
   }

   override fun analyze(image: ImageProxy) {

       val buffer = image.planes[0].buffer
       val data = buffer.toByteArray()
       val pixels = data.map { it.toInt() and 0xFF }
       val luma = pixels.average()

       listener(luma)

       image.close()
   }
}
```
  
   ②接下来更新startCamera()，将以下代码添加到 imageCapture 代码下方。
   
```
 val imageAnalyzer = ImageAnalysis.Builder()
   .build()
   .also {
       it.setAnalyzer(cameraExecutor, LuminosityAnalyzer { luma ->
           Log.d(TAG, "Average luminosity: $luma")
       })
   }

```
  
  ③ 更新 cameraProvider 上的 bindToLifecycle() 调用，以包含 imageAnalyzer。
  
  （2）实验代码：

```
cameraProvider.bindToLifecycle(
   this, cameraSelector, preview, imageCapture, imageAnalyzer)

```
  
  ④立即运行应用！它会大约每秒在 logcat 中生成一个类似于下面的消息。实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.8.png" alt="avatar" style="zoom:50%; width:750px" />

  ### 9.实现 VideoCapture 用例（拍摄视频）

- #### 思路

  ①将以下代码复制到captureVideo() 方法：该方法可以控制 VideoCapture 用例的启动和停止。

```
// Implements VideoCapture use case, including start and stop capturing.
private fun captureVideo() {
   val videoCapture = this.videoCapture ?: return

   viewBinding.videoCaptureButton.isEnabled = false

   val curRecording = recording
   if (curRecording != null) {
       // Stop the current recording session.
       curRecording.stop()
       recording = null
       return
   }

   // create and start a new recording session
   val name = SimpleDateFormat(FILENAME_FORMAT, Locale.US)
              .format(System.currentTimeMillis())
   val contentValues = ContentValues().apply {
       put(MediaStore.MediaColumns.DISPLAY_NAME, name)
       put(MediaStore.MediaColumns.MIME_TYPE, "video/mp4")
       if (Build.VERSION.SDK_INT > Build.VERSION_CODES.P) {
           put(MediaStore.Video.Media.RELATIVE_PATH, "Movies/CameraX-Video")
       }
   }

   val mediaStoreOutputOptions = MediaStoreOutputOptions
       .Builder(contentResolver, MediaStore.Video.Media.EXTERNAL_CONTENT_URI)
       .setContentValues(contentValues)
       .build()
   recording = videoCapture.output
       .prepareRecording(this, mediaStoreOutputOptions)
       .apply {
           if (PermissionChecker.checkSelfPermission(this@MainActivity,
                   Manifest.permission.RECORD_AUDIO) ==
               PermissionChecker.PERMISSION_GRANTED)
           {
               withAudioEnabled()
           }
       }
       .start(ContextCompat.getMainExecutor(this)) { recordEvent ->
           when(recordEvent) {
               is VideoRecordEvent.Start -> {
                   viewBinding.videoCaptureButton.apply {
                       text = getString(R.string.stop_capture)
                       isEnabled = true
                   }
               }
               is VideoRecordEvent.Finalize -> {
                   if (!recordEvent.hasError()) {
                       val msg = "Video capture succeeded: " +
                           "${recordEvent.outputResults.outputUri}"
                       Toast.makeText(baseContext, msg, Toast.LENGTH_SHORT)
                            .show()
                       Log.d(TAG, msg)
                   } else {
                       recording?.close()
                       recording = null
                       Log.e(TAG, "Video capture ends with error: " +
                           "${recordEvent.error}")
                   }
                   viewBinding.videoCaptureButton.apply {
                       text = getString(R.string.start_capture)
                       isEnabled = true
                   }
               }
           }
       }
}
```
 
  ②在 startCamera() 中，将以下代码放置在 preview 创建行之后。这将创建 VideoCapture 用例。
```
val recorder = Recorder.Builder()
   .setQualitySelector(QualitySelector.from(Quality.HIGHEST))
   .build()
videoCapture = VideoCapture.withOutput(recorder)
```
  ③ 将 Preview + VideoCapture 用例绑定到生命周期相机。仍在 startCamera() 内，将 cameraProvider.bindToLifecycle() 调用替换为以下代码：
```
// Bind use cases to camera
cameraProvider.bindToLifecycle(this, cameraSelector, preview, videoCapture)
```
  ④构建并运行项目,录制一些剪辑,视频录制测试可以看到视频被成功保存至媒体库。实验结果如下图：
- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.9.png" alt="avatar" style="zoom:50%; width:750px" />

- <img src="https://github.com/17515424731/Project/blob/main/image/2.2.10.png" alt="avatar" style="zoom:50%; width:750px" />
