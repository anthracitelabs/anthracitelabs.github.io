# _Android_

‘Step by step guide for Android development’
In this guide you will develop an Android application from scratch - _without_ an IDE and also without Gradle.

## Prepare your environment

**I have jdk-21.0.7 installed and JAVA_HOME defined, also jdk bin folder added to PATH variable. Android command line tools only downloaded. https://developer.android.com/studio/index.html#command-line-tools-only
Unzip android command line tools into ANDROID SDK folder. List installed and available packages:**

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

* android_sdk/cmdline-tools/version/bin/
* android_sdk/build-tools/version/
* android_sdk/platform-tools/
* android_sdk/emulator/

Recommended settings for environment: (env variables reference : https://developer.android.com/tools/variables#set)	

* _ANDROID_HOME_ - the path to the SDK installation directory
* _ANDROID_USER_HOME_ - the path to the user preferences directory for tools that are part of the Android SDK. Defaults to $HOME/.android/

add the following to PATH

* _ANDROID_HOME_/build-tools/version
* _ANDROID_HOME_/cmdline-tools/version/bin
* _ANDROID_HOME_/platform-tools
  
### Sample Libgdx Project Android Compilation

under project/libs folder download

* desugar_jdk_libs-2.1.5.jar
* gdx-1.13.1.jar
* gdx-backend-android-1.13.1.jar
* gdx-platform-1.13.1-natives-arm64-v8a.jar
* gdx-platform-1.13.1-natives-armeabi-v7a.jar
* gdx-platform-1.13.1-natives-x86_64.jar

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

---

## Epilogue

So it is possible to develop an Android application just by using the command line without any need for an IDE. This enables a much more comprehensive learning process.

