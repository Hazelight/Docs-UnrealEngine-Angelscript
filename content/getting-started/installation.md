+++
title = "Installation"
weight = 10
+++

# Installation
## 1. Getting Unreal Engine Source Access
The angelscript integration requires direct changes to unreal engine code to work.
You will need to get access to unreal engine source code to be able to access the 
version of the engine used in this project.

To get source code access, follow this guide from Epic Games:
[https://www.unrealengine.com/en-US/ue-on-github](https://www.unrealengine.com/en-US/ue-on-github)


## 2. Downloading UnrealEngine-Angelscript
After getting access to unreal source code, clone the [UnrealEngine-Angelscript GitHub Repository](https://github.com/Hazelight/UnrealEngine-Angelscript)
and build the unreal editor using visual studio as usual.

Note that any additional plugins you want to add to the engine need to be built from source.
This fork is not be compatible with pre-built binary plugins.


## 3. Installing the Visual Studio Code Extension
A visual studio code extension with auto-completion and debugging support is available.

Install visual studio code from [https://code.visualstudio.com/](https://code.visualstudio.com/).

Then install the [Unreal Angelscript Extension](https://marketplace.visualstudio.com/items?itemName=Hazelight.unreal-angelscript)
from the marketplace, or search for 'Unreal Angelscript' in the extensions sidebar within Visual Studio Code.