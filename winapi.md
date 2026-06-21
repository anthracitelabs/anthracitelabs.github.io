---
layout: default
---

# Writing a Simple Windows Application using Win32 API 

<p class="topshort">This simple Windows application creates and shows a window.</p>

![sea, underwater fish](/images/fish.png){:style="width: 80%; display: block;float: none;margin-left: auto;margin-right: auto;margin-top: 50px;margin-bottom: 50px;"} 


## 1. Getting your environment ready


You can use any editor or IDE you like to follow this tutorial. I use a text editor (vim) to edit source files and command line to compile them into the final executable. You only need to download [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019).


Alternatively, to use Visual Studio as your source editor/debugger do the following in Visual Studio installation:


- In the Workloads section, select only Desktop development with C++
- You can further reduce total space required by leaving only the following tools (### stands for current version number of the tools):
- MSVC v### - VS 2019 C++ x64/x86 build tools
- C++/CLI support for v###
- Windows 10 SDK (highest version available)
- IntelliCode (If you are planning on using Visual Studio as your text editor)


Let's see if we can compile a simple Windows program. Below code includes the entry point (WinMain) for our program. Create a text file named **main.cpp** which will be our only source file, for now.


~~~~~~~~ C++ 
#include <windows.h>

int CALLBACK 
WinMain (HINSTANCE hInstance,
         HINSTANCE hPrevInstance,
         LPSTR     lpCmdLine,
         int       nShowCmd)
{
    return (0);
}
~~~~~~~~


Use the Visual Studio compiler `cl` to compile the source file. Note that `cl` command also runs the linker by default.

```console
W:\mycode>cl main.cpp
```


Windows has its own set of graphics libraries [as seen here](https://learn.microsoft.com/en-us/windows/win32/learnwin32/overview-of-the-windows-graphics-architecture)
