---
layout: article
title: Use Visual Studio for Unity
category: Dev_Tools
tag: [IDE, Unity]
date: 2017-10-23
---

### Configure Visual Studio for Unity projects
* Set Visual Studio as IDE for the project
  * From Unity Editor, access Edit -> Preferences -> External Tools
  * In 'External Script Editor', select Visual Studio
  * Enable 'Editor Attaching' option
* Set the build as development and a target
  * From Unity Editor, access File -> Build Settings
  * Select a platform
  * Enable 'Development Build' and 'Script Debugging' options
* Connect Visual Studio to Unity
  * From Visual Studio, access Debug -> Attach Unity Debugger
  * Select the Unity project to connect to throught a UDP connection
* To debug games running in Unity Web Player
  * In Unity Web Player, open context menu while maintaining Alt
  * Access "Release Channel"
  * Enable "Development" option
  
### VSUT Commands
* To open Unity API documentation about specific subject
  * Ctrl + Alt + M , Ctrl + H
* To generate method definition to override from Monobehavior
  * To open wizard : Ctrl + Shift + M
  * To open quick search : Ctrl + Shift + Q
  
### Use Application Lifecycle Management to develop, test, and deploy games


## References
[Visual Studio Tools for Unity doc](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity)  
[Application Lifecycle Management doc](https://docs.microsoft.com/en-us/visualstudio/cross-platform/application-lifecycle-management-alm-with-unity-apps)
