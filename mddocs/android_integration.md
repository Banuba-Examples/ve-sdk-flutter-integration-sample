# Android Integration Guide into Flutter project

An integration and customization of Banuba Video Editor SDK is implemented in **android** directory 
of your Flutter project using native Android development process.

## Basic
The following steps help to complete basic integration into your Flutter project.

<ins>All changes are made in **android** directory.</ins>
1. __Set Banuba Video Editor SDK token__  
   Set Banuba token in [resources](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/src/main/res/values/string.xml#L5).<br></br>
   To get access to your trial, please, get in touch with us by [filling a form](https://www.banuba.com/video-editor-sdk) on our website. Our sales managers will send you the trial token.<br>
   :exclamation: The token **IS REQUIRED** to run sample and an integration in your app.<br></br>

2. __Add Banuba Video Editor SDK dependencies__ </br>
   Add Banuba repositories in main gradle file to get Video Editor SDK and AR Cloud dependencies.
    ```groovy
        maven {
            name = "GitHubPackages"
            url = uri("https://maven.pkg.github.com/Banuba/banuba-ve-sdk")
            credentials {
                username = "Banuba"
                password = ""
            }
        }
        maven {
            name = "ARCloudPackages"
            url = uri("https://github.com/Banuba/banuba-ar")
            credentials {
                username = "Banuba"
                password = ""
            }
        }
    ```
   [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/build.gradle#L16)</br><br>

   Add Video Editor SDK dependencies in app gradle file.
    ```groovy
        def banubaSdkVersion = '1.24.2'
        implementation "com.banuba.sdk:ffmpeg:4.4"
        implementation "com.banuba.sdk:camera-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:camera-ui-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:core-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:core-ui-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-flow-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-timeline-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-ui-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-gallery-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-effects-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:effect-player-adapter:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ar-cloud:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-audio-browser-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:banuba-token-storage-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-export-sdk:${banubaSdkVersion}"
        implementation "com.banuba.sdk:ve-playback-sdk:${banubaSdkVersion}"
   ```

    [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/build.gradle#L76)</br><br>
3. __Add SDK Initializer class__ </br>
     Add [BanubaVideoEditorSDK.kt](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/src/main/kotlin/com/banuba/flutter/flutter_ve_sdk/BanubaVideoEditorSDK.kt) file.</br>
     This class helps to initialize and customize Video Editor SDK.</br><br>

4. __Initialize Video Editor SDK in your application__ </br>
     Use ```BanubaVideoEditorSDK().initialize(...)``` in your ```Application.onCreate()``` method to initialize Video Editor SDK.</br>
     [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/src/main/kotlin/com/banuba/flutter/flutter_ve_sdk/SampleApp.kt#L10)</br><br>

5. __Setup platform channel to start Video Editor SDK__  
     Add handler to your ```FlutterActivity``` to listen to calls from Flutter side to start Video Editor.</br>
     ```kotlin
      MethodChannel(
            appFlutterEngine.dartExecutor.binaryMessenger,
            "CHANNEL"
        ).setMethodCallHandler { call, result ->
            // Listen to call from Flutter side
            if (call.method.equals("StartBanubaVideoEditor")) {
                exportVideoChanelResult = result
                startVideoEditorSDK()
            } else {
                result.notImplemented()
            }
        }
     ```
     [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/src/main/kotlin/com/banuba/flutter/flutter_ve_sdk/MainActivity.kt#L31).<br></br>
     Use ```VideoCreationActivity.startFromCamera(...)``` method to start Video Editor SDK from Camera screen.
     Once Video Editor SDK starts you can observe export video results.</br>
     [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/src/main/kotlin/com/banuba/flutter/flutter_ve_sdk/MainActivity.kt#L46)</br><br>
     Find more information about platform channels in [Flutter developer documentation](https://docs.flutter.dev/development/platform-integration/platform-channels).</br><br>

6. __Update AndroidManifest.xml__ </br>
     Add ```VideoCreationActivity``` in your AndroidManifest.xml file. The Activity is used to bring together a number of screens in a certain flow.</br>
     [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/android/app/src/main/AndroidManifest.xml#L47)</br><br>

7. __Add assets and resources__</br>
      1. [bnb-resources](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/assets/bnb-resources) to use build-in Banuba AR and Lut effects.
      Using Banuba AR ```assets/bnb-resources/effects``` requires [Face AR product](https://docs.banuba.com/face-ar-sdk-v1). Please contact Banuba Sales managers to get more AR effects.<br></br>
   
      2. [color](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/color),
      [drawable](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable),
      [drawable-hdpi](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable-hdpi),
      [drawable-ldpi](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable-ldpi),
      [drawable-mdpi](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable-mdpi),
      [drawable-xhdpi](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable-xhdpi),
      [drawable-xxhdpi](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable-xxhdpi),
      [drawable-xxxhdpi](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/drawable-xxxhdpi) are visual assets used in views and added in the sample for simplicity. You can use your specific assets.<br></br>
   
      3. [values](https://github.com/Banuba/ve-sdk-flutter-integration-sample/tree/main/android/app/src/main/res/values) to use colors and themes. Theme ```VideoCreationTheme``` and its styles use resources in **drawable** and **color** directories.<br></br>

8. __Start Video Editor SDK__ </br>
    Use ```platform.invokeMethod``` to start Video Editor SDK from Flutter.</br>
    ```dart
       Future<void> _startVideoEditorAndroid() async {
          try {
            final result = await platform.invokeMethod('StartBanubaVideoEditor');
            debugPrint('Result: $result ');
          } on PlatformException catch (e) {
            debugPrint("Error: '${e.message}'.");
         }
      }
   ```
    [See example](https://github.com/Banuba/ve-sdk-flutter-integration-sample/blob/main/lib/main.dart#L111).</br>

   
## What is next?

We have covered a basic process of Banuba Video Editor SDK integration into your Flutter project. 
More integration details and customization options you will find in [Banuba Video Editor SDK Android Integration Sample](https://github.com/Banuba/ve-sdk-android-integration-sample).