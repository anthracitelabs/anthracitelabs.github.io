---
layout: default
---

# Digital images

<p class="topshort">Android development in pure C style without using Gradle. You don't need to depend on it as it is a heavy bloat requires internet connection and downloads a ton of other dependencies, etc.</p>

![sea, underwater fish](/images/fish.png){:style="width: 80%; display: block;float: none;margin-left: auto;margin-right: auto;margin-top: 50px;margin-bottom: 50px;"}

In a hole in the ground there lived a hobbit. Not a nasty, dirty, wet hole, filled with the ends
of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to
eat: it was a [hobbit-hole](https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"), and that means comfort.
And it's _really_ perfect!


**Ten declarations, a `@media` block, and one attribute.**

That's how much CSS and HTML it took to turn that [old](https://anthracitelabs.io/) into this finally perfect masterpiece. It's so simple and it still has all the glory:

* It's _finally_ lightweight and loads fast;
* _Finally_ fits on all your screens;
* _Finally_ looks the same in all your browsers;
* _Won't_ bleach your eyeballs at night if your browser thinks you like dark things;
* The site is _still_ accessible to everyone that visits your site;
* It's _still_ legible and gets your point across.

### And guess what:

You never knew it, but it's easy to improve reliability on your site. Here's how.

~~~c
// You never knew it, but it’s easy to improve reliability on your site. Here’s how.
#include <stdio.h>

static float foo;

int main()
{
  printf("Hello world!");
  return (0);
}
~~~

## A little more accessibility

I know you love marginalization, but you should take care of people.
The <abbr title="HyperText Markup Language">HTML</abbr> tag doesn't include the [`lang` attribute](https://www.w3.org/TR/html5/dom.html#the-lang-and-xml:lang-attributes) in your webpage.
You ever listen to a [wrong language screen reader lecture](https://www.youtube.com/watch?v=NP94u7y_KkQ)? What a bad experience.

## Typography matters

[Don't be dumb with typography](https://smartquotesforsmartpeople.com/). Don't let improper marks and quotes make their way onto websites because of dumb defaults in applications and CMSs.  
And did you let the browser choose the font? Don't make me read such text with _Times New Roman_.

## License, matters

Do you revendicate intellectual property? No. So you must [choose a public license](https://choosealicense.com/).  
And as you'll never be perfect, let others [improve your work](https://github.com/anthracitelabs/anthracitelabs.io) or [do what they want to](https://github.com/anthracitelabs.io/blob/main/LICENSE).

## Yes, this is plain simple as it should be

What I'm saying is that it's so, so simple to make sites easier to read. Websites are broken by default—they are functional, high-performing, and accessible, but they're also ugly. You and all the other web designers out there need to make them not total failure.

> "Don't ignore stupid things or you will stay at the mediocre level." 
> — Brad Hollande

<kbd>Strg+X</kbd>  
<kbd>Strg</kbd> + <kbd>X</kbd>  
<kbd>Or any other text</kbd>

This is a text with <sup>SuperScript</sup>  
You can use it for footnotes<sup>1</sup>

---

<sup>[1]: See [footnotes](../02-footer/footnotes)</sup>

---

## Epilogue

Inspired by the geniuses behind ultimate web sites.

This page—that isn't a total content—was licensed under [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

---

<sup>[1]: Hello World</sup>  
<sup>[2]: Hello Internet</sup>

## Now back to business

# Sample Android Hello World Project 

First, you need to install java, in my case, I install the headless version because I don’t use graphics (only command line):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo apt-get install openjdk-8-jdk-headless
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Then download the last SDK tools of Android
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
mkdir -p /opt/android-sdk
unzip sdk-tools-linux-3859397.zip -d /opt/android-sdk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now, we have to install platform tools (which contain ADB), an Android API and build tools.
In fact, if you are on Debian, you can avoid installing platform-tools package and only install ADB like that:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo apt-get install adb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this example, I want to compile a simple HelloWorld.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
export PROJ=path/to/HelloAndroid
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, we need generate the R.java file which is necessary for our code:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd /opt/android-sdk/build-tools/26.0.1/
./aapt package -f -m -J $PROJ/src -M $PROJ/AndroidManifest.xml -S $PROJ/res -I /opt/android-sdk/platforms/android-19/android.jar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-m instructs aapt to create directories under the location specified by -J
-J specifies where the output goes. Saying -J src will create a file like src/com/example/helloandroid/R.java
-S specifies where is the res directory with the drawables, layouts, etc.
-I tells aapt where the android.jar is. You can find yours in a location like android-sdk/platforms/android-<API level>/android.jar

Now, we have to compile the .java files:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd /path/to/AndroidHello
javac -d obj -classpath src -bootclasspath /opt/android-sdk/platforms/android-19/android.jar src/com/example/helloandroid/*.java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have use an external, add it the classpath:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
javac -d obj -classpath "src:libs/<your-lib>.jar" -bootclasspath /opt/android-sdk/platforms/android-19/android.jar src/com/example/helloandroid/*.java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The compiled .class files are in obj folder, but Android can’t read them. We have to translate them in a file called “classes.dex” which will be read by the dalvik Android runtime:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd /opt/android-sdk/build-tools/26.0.1/
./dx --dex --output=$PROJ/bin/classes.dex $PROJ/obj
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

But if you use external libraries, do rather:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
./dx --dex --output=$PROJ/bin/classes.dex $PROJ/*.jar $PROJ/obj
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have the error UNEXPECTED TOP-LEVEL EXCEPTION, it can be because you use old build tools and DX try to translate java 1.7 rather than 1.8. To solve the problem, you have to specify 1.7 java version in the previous javac command:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd /path/to/AndroidHello
javac -d obj -source 1.7 -target 1.7 -classpath src -bootclasspath /opt/android-sdk/platforms/android-19/android.jar src/com/example/helloandroid/*.java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The -source option specify the java version of your source files. Note that we can use previous versions of Java even we use OpenJDK 8 (or 1.8).

We can now put everything in an APK:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
./aapt package -f -m -F $PROJ/bin/hello.unaligned.apk -M $PROJ/AndroidManifest.xml -S $PROJ/res -I /opt/android-sdk/platforms/android-19/android.jar
cp $PROJ/bin/classes.dex .
./aapt add $PROJ/bin/hello.unaligned.apk classes.dex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Be aware: until now, we used three AAPT commands, the first and the second one are similar but they don’t do the same. You have to copy the classes.dex file at the root of project like above! Otherwise, AAPT won’t put this file at right place in the APK archive (because an APK is like a .zip file).
The generated package can’t be installed by Android because it’s unaligned and unsigned.
If you want, you can check the content of the package like this:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
./aapt list $PROJ/bin/hello.unaligned.apk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sign the package
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
keytool -genkeypair -validity 365 -keystore mykey.keystore -keyalg RSA -keysize 2048
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Just answer the questions and put a password.

You can sign an APK like this:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
./apksigner sign --ks mykey.keystore $PROJ/bin/hello.apk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Align the package
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
./zipalign -f 4 $PROJ/bin/hello.unaligned.apk $PROJ/bin/hello.apk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Alignment increase the performance of the application and may reduce memory use.

Test the application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
adb install $PROJ/bin/hello.apk
adb shell am start -n com.example.helloandroid/.MainActivity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

sh script version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
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
