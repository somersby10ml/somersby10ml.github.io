---
title: "3 ways to add source to files compiled in .NET"
categories:
  - windows
  
tags:
  - trick

---

This article explains how to add sources to a binary compiled as a .NET (C#/Net) file.

[ILSpy](//github.com/icsharpcode/ILSpy){:target="_blank"}  
[dotPeek]( //www.jetbrains.com/ko-kr/decompiler/){:target="_blank"}  
[dnSpy](//github.com/dnSpy/dnSpy/releases){:target="_blank"}  

## before you start
It is recommended to unobfuscate with **de4net** before starting.  
[https://github.com/de4dot/de4dot](https://github.com/de4dot/de4dot){:target="_blank"}

# using Tools
## Method 1 
using dnspy  
[github.com/dnSpy/dnSpy/releases](github.com/dnSpy/dnSpy/releases){:target="_blank"}

![Image1](/assets/img/addsourcefromnet/image1.png)  
There are several options you can modify.  


# Programming C#
## Method 1
Using [Mono.Cecil](https://www.mono-project.com/docs/tools+libraries/libraries/Mono.Cecil/){:target="_blank"} or **Nuget**  
reference [Mono.Cecil.Inject](https://github.com/ghorsington/Mono.Cecil.Inject){:target="_blank"}  

1. create `inject.cs` and compile
1. copy method
1. insert target file
1. redefinition  


Simple Code [ILCodeInjector](https://github.com/somersby10ml/ILCodeInjector){:target="_blank"}

## Method 2
1. Create New Project C#.
1. add Resource from target binary.
1. change this icon from target icon. [C# Icon Explorer](https://github.com/TsudaKageyu/IconExtractor){:target="_blank"}
1. Copy Assembly and versionInfo from target
1. set main source
    ```csharp
    // before 
    ResourceManager ResManager = new ResourceManager("myResources", Assembly.GetExecutingAssembly());
    byte[] mainByteArray = (byte[])ResManager.GetObject("aaaa");
    Assembly assembly = Assembly.Load(mainByteArray);
    assembly.EntryPoint.Invoke(null, new object[0]); // run Program
    // after (exit program)
    ```
read Assembly files and executed from resources.  
Tasks can be added before or after program startup.  