---
layout: default
---

# Manually Build an Android Hello World Project

<p class="topshort">Android development is possible without using Gradle. You don't need to depend on it as it is a heavy bloat 
requires internet connection and downloads a ton of other dependencies, etc. You build faster and know what is going on.
</p>

![sea, underwater fish](/images/fish.png){:style="width: 80%; display: block;float: none;margin-left: auto;margin-right: auto;margin-top: 50px;margin-bottom: 50px;"} 

First, you need to install java, in my case, I install the headless version because I don’t use graphics (only command line):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
sudo apt-get install openjdk-8-jdk-headless
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Then download the last SDK tools of Android
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
mkdir -p /opt/android-sdk
unzip sdk-tools-linux-3859397.zip -d /opt/android-sdk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now, we have to install platform tools (which contain ADB), an Android API and build tools.
In fact, if you are on Debian, you can avoid installing platform-tools package and only install ADB like that:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
sudo apt-get install adb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this example, I want to compile a simple HelloWorld.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
mkdir HelloAndroid
cd HelloAndroid
mkdir -p src/com/example/helloandroid
mkdir obj
mkdir bin
mkdir -p res/layout
mkdir res/values
mkdir res/drawable
mkdir libs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make the file src/com/example/helloandroid/MainActivity.java and put that inside:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Java
package com.example.helloandroid;

import android.app.Activity;
import android.os.Bundle;

public class MainActivity extends Activity {
   @Override
   protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
   }
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make the strings.xml file in the res/values folder. It contains all the text that your application uses:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Xml
<resources>
   <string name="app_name">A Hello Android</string>
   <string name="hello_msg">Hello Android!</string>
   <string name="menu_settings">Settings</string>
   <string name="title_activity_main">MainActivity</string>
</resources>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The activity_main.xml is a layout file which have to be in res/layout:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android" xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent" >
   
   <TextView
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_centerHorizontal="true"
      android:layout_centerVertical="true"
      android:text="@string/hello_msg"
      tools:context=".MainActivity" />
</RelativeLayout>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You also have to add the file AndroidManifest.xml at the root:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Xml
<?xml version='1.0'?>
<manifest xmlns:a='http://schemas.android.com/apk/res/android' package='com.example.helloandroid' a:versionCode='0' a:versionName='0'>
    <application a:label='A Hello Android'>
        <activity a:name='com.example.helloandroid.MainActivity'>
             <intent-filter>
                <category a:name='android.intent.category.LAUNCHER'/>
                <action a:name='android.intent.action.MAIN'/>
             </intent-filter>
        </activity>
    </application>
</manifest>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Build the code
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
export PROJ=path/to/HelloAndroid
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, we need generate the R.java file which is necessary for our code:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
cd /opt/android-sdk/build-tools/26.0.1/
./aapt package -f -m -J $PROJ/src -M $PROJ/AndroidManifest.xml -S $PROJ/res -I /opt/android-sdk/platforms/android-19/android.jar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-m instructs aapt to create directories under the location specified by -J
-J specifies where the output goes. Saying -J src will create a file like src/com/example/helloandroid/R.java
-S specifies where is the res directory with the drawables, layouts, etc.
-I tells aapt where the android.jar is. You can find yours in a location like android-sdk/platforms/android-<API level>/android.jar

Now, we have to compile the .java files:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
cd /path/to/AndroidHello
javac -d obj -classpath src -bootclasspath /opt/android-sdk/platforms/android-19/android.jar src/com/example/helloandroid/*.java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have use an external, add it the classpath:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
javac -d obj -classpath "src:libs/<your-lib>.jar" -bootclasspath /opt/android-sdk/platforms/android-19/android.jar src/com/example/helloandroid/*.java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The compiled .class files are in obj folder, but Android can’t read them. We have to translate them in a file called “classes.dex” which will be read by the dalvik Android runtime:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
cd /opt/android-sdk/build-tools/26.0.1/
./dx --dex --output=$PROJ/bin/classes.dex $PROJ/obj
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

But if you use external libraries, do rather:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
./dx --dex --output=$PROJ/bin/classes.dex $PROJ/*.jar $PROJ/obj
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have the error UNEXPECTED TOP-LEVEL EXCEPTION, it can be because you use old build tools and DX try to translate java 1.7 rather than 1.8. To solve the problem, you have to specify 1.7 java version in the previous javac command:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
cd /path/to/AndroidHello
javac -d obj -source 1.7 -target 1.7 -classpath src -bootclasspath /opt/android-sdk/platforms/android-19/android.jar src/com/example/helloandroid/*.java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The -source option specify the java version of your source files. Note that we can use previous versions of Java even we use OpenJDK 8 (or 1.8).

We can now put everything in an APK:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
./aapt package -f -m -F $PROJ/bin/hello.unaligned.apk -M $PROJ/AndroidManifest.xml -S $PROJ/res -I /opt/android-sdk/platforms/android-19/android.jar
cp $PROJ/bin/classes.dex .
./aapt add $PROJ/bin/hello.unaligned.apk classes.dex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Be aware: until now, we used three AAPT commands, the first and the second one are similar but they don’t do the same. You have to copy the classes.dex file at the root of project like above! Otherwise, AAPT won’t put this file at right place in the APK archive (because an APK is like a .zip file).
The generated package can’t be installed by Android because it’s unaligned and unsigned.
If you want, you can check the content of the package like this:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
./aapt list $PROJ/bin/hello.unaligned.apk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sign the package
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
keytool -genkeypair -validity 365 -keystore mykey.keystore -keyalg RSA -keysize 2048
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Just answer the questions and put a password.

You can sign an APK like this:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
./apksigner sign --ks mykey.keystore $PROJ/bin/hello.apk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Align the package
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
./zipalign -f 4 $PROJ/bin/hello.unaligned.apk $PROJ/bin/hello.apk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Alignment increase the performance of the application and may reduce memory use.

Test the application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
adb install $PROJ/bin/hello.apk
adb shell am start -n com.example.helloandroid/.MainActivity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

sh script version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Bash
#!/bin/bash

set -e

AAPT="/path/to/android-sdk/build-tools/23.0.3/aapt"
DX="/path/to/android-sdk/build-tools/23.0.3/dx"
ZIPALIGN="/path/to/android-sdk/build-tools/23.0.3/zipalign"
APKSIGNER="/path/to/android-sdk/build-tools/26.0.1/apksigner" # /!\ version 26
PLATFORM="/path/to/android-sdk/platforms/android-19/android.jar"

echo "Cleaning..."
rm -rf obj/*
rm -rf src/com/example/helloandroid/R.java

echo "Generating R.java file..."
$AAPT package -f -m -J src -M AndroidManifest.xml -S res -I $PLATFORM

echo "Compiling..."
javac -d obj -classpath src -bootclasspath $PLATFORM -source 1.7 -target 1.7 src/com/example/helloandroid/MainActivity.java
javac -d obj -classpath src -bootclasspath $PLATFORM -source 1.7 -target 1.7 src/com/example/helloandroid/R.java

echo "Translating in Dalvik bytecode..."
$DX --dex --output=classes.dex obj

echo "Making APK..."
$AAPT package -f -m -F bin/hello.unaligned.apk -M AndroidManifest.xml -S res -I $PLATFORM
$AAPT add bin/hello.unaligned.apk classes.dex

echo "Aligning and signing APK..."
$APKSIGNER sign --ks mykey.keystore bin/hello.unaligned.apk
$ZIPALIGN -f 4 bin/hello.unaligned.apk bin/hello.apk

if [ "$1" == "test" ]; then
	echo "Launching..."
	adb install -r bin/hello.apk
	adb shell am start -n com.example.helloandroid/.MainActivity
fi
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






# Android another way

Step by step guide for Android development   
In this guide you will develop an Android application from scratch - without an IDE and also without Gradle.

Prepare your environment
==============================================================

I have jdk-21.0.7 installed and JAVA_HOME defined, also jdk bin folder added to PATH variable. Gradle downloaded
Android command line tools only downloaded. https://developer.android.com/studio/index.html#command-line-tools-only
Unzip android command line tools into ANDROID SDK folder. List installed and available packages: 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager --list [options]
[--channel=channel_id] // Channels: 0 (stable), 1 (beta), 2 (dev), or 3 (canary)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
Execute the following to list stable packages:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager --list --channel=0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## SDK Installation

To install packages: 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager packages [options] 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The packages argument is an SDK-style path, as shown with the --list command, wrapped in quotes. For example, "build-tools;34.0.0" or "platforms;android-33". To install Android SDK Platform 36, Android SDK Build-Tools 36 and Android SDK Platform-Tools 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager "platforms;android-36" "build-tools;36.0.0" "platform-tools"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SDK folder structure:

- android_sdk/cmdline-tools/version/bin/
- android_sdk/build-tools/version/
- android_sdk/platform-tools/
- android_sdk/emulator/ 

Recommended settings for environment: (env variables reference : https://developer.android.com/tools/variables#set)	

- ANDROID_HOME - the path to the SDK installation directory
- ANDROID_USER_HOME - the path to the user preferences directory for tools that are part of the Android SDK. Defaults to $HOME/.android/

add the following to PATH

- ANDROID_HOME/build-tools/version
- ANDROID_HOME/cmdline-tools/version/bin
- ANDROID_HOME/platform-tools 


# Sample Libgdx Project Android Compilation
--------------------------------------------

under project/libs folder download

- desugar_jdk_libs-2.1.5.jar
- gdx-1.13.1.jar
- gdx-backend-android-1.13.1.jar
- gdx-platform-1.13.1-natives-arm64-v8a.jar
- gdx-platform-1.13.1-natives-armeabi-v7a.jar
- gdx-platform-1.13.1-natives-x86_64.jar

Below is the batch script for manual compilation steps

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ bat
@echo off

call clean.bat
SET ANDROID_JAR="%ANDROID_HOME%\platforms\android-36\android.jar"

:: build core module
cd core
mkdir build
mkdir build\lib

dir /s /B *.java > sources.txt
call javac -d build -classpath ..\libs\gdx-1.13.1.jar;..\libs\armeabi-v7a.jar @sources.txt
call jar cvf build\lib\core.jar -C build .

::echo "--------------------------------------"
::echo "core compile complete..."
::echo "--------------------------------------"

:: build android module
cd ..\android
mkdir build
mkdir build\gen
mkdir build\compiled-res
mkdir bin

call aapt2 compile --dir res -o build/compiled-res

SET flat_files=
for /f "tokens=* delims=" %%i in ('dir /s /B *.flat') do ( call set "flat_files=%%flat_files%% %%i" )

call aapt2 link -o build/app-unsigned.apk -I %ANDROID_JAR% --manifest AndroidManifest.xml --min-sdk-version 26 --target-sdk-version 36 --version-code 123 --java build\gen --debug-mode %flat_files%

::echo "--------------------------------------"
::echo "aapt2 compile-link complete..."
::echo "--------------------------------------"

dir /s /B *.java > sources.txt
call javac -d bin -classpath %ANDROID_JAR%;..\core\build\lib\core.jar;..\libs\gdx-1.13.1.jar;..\libs\gdx-backend-android-1.13.1.jar;..\libs\armeabi-v7a.jar @sources.txt

::echo "--------------------------------------"
::echo "android compile complete..."
::echo "--------------------------------------"

call jar cvf classes.jar -C bin .

call d8 classes.jar ..\core\build\lib\core.jar ..\libs\*.jar --output bin --lib %ANDROID_JAR% --min-api 26 --debug

::echo "--------------------------------------"
::echo "dex complete..."
::echo "--------------------------------------"

cd bin
call "C:\Program Files\7-Zip\7z.exe" a -tzip ..\build\app-unsigned.apk classes.dex
cd..

call "C:\Program Files\7-Zip\7z.exe" a -tzip build\app-unsigned.apk lib

::echo "--------------------------------------"
::echo "added .so files to apk..."
::echo "--------------------------------------"

call zipalign -f 4 build\app-unsigned.apk build\aligned.apk
::call zipalign -P 16 -f -v 4 build\app-unsigned.apk build\aligned.apk
::call zipalign -c -P 16 -v 4 build\aligned.apk
call apksigner sign --ks ToyKey.keystore --ks-key-alias HelloJNIKey --ks-pass pass:armena --out build\final.apk build\aligned.apk
::echo "--------------------------------------"
::echo "apk sign complete..."
::echo "--------------------------------------"
call adb install -r build\final.apk

cd..
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

to generate key 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
%JAVABIN%\keytool  -genkey -v -keystore  my-release-key.keystore -alias alias_name  -keyalg RSA -keysize 2048  -validity 10000
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Installing the app to physical device
--------------------------------------------------------------

install emulator:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager "emulator"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

find a system image using the following command:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager.bat --list
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

install system image:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
sdkmanager.bat "system-images;android-30;google_apis_playstore;x86_64"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

list available devices with their ids:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
avdmanager.bat list device
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

create device with name test and device id 39 using the system image we installed above:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
avdmanager create avd -n test -k "system-images;android-30;google_apis_playstore;x86_64" -d 39
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

run the newly created device using emulator tool:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
emulator @test -skin 768x1280
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Installation is done via adb command to specific devices..
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
adb install <path/to/your/app.apk>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Lists all Android Virtual Devices (AVDs) available on your development machine. This command is useful for identifying the emulators you can launch to test your application.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
emulator -list-avds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
Start a specific Android Virtual Device (AVD).
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
emulator -avd avd_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This command saves the logcat output to logcat.txt in the current directory. The d option dumps the current logcat buffer.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
adb logcat -d > logcat.txt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	
Use adb qualified names of all apps/packages. If you forget what your qualified app name, but it's installed on the device, then:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
adb shell pm list packages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

And then
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
	adb shell monkey -p your.app.name -v 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
(to be more precise, this runs the app and sends 1 random input to it)


The Last Word
============================================================== 
So it is possible to develop an Android application just by using the command line without any need for an IDE. This enables a much more comprehensive learning process.



