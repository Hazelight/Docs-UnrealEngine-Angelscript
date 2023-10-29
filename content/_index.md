+++
title = "Index"
+++

# Unreal Engine Angelscript
UnrealEngine-Angelscript is a set of engine modifications and a plugin for UE5 that integrates a
full-featured scripting language.
It is actively developed by [Hazelight](http://hazelight.se), creators of [It Takes Two](https://www.ea.com/games/it-takes-two), which was shipped with the majority of its gameplay written in angelscript.
More recently, [The Talos Principle 2](https://www.thetalosprinciple.com/) was also shipped by [Croteam](http://www.croteam.com/) using UnrealEngine-Angelscript.

The unreal plugin that integrates angelscript is open source, and has received contributions from studios in Stockholm and globally.

See [Scripting Introduction](getting-started/introduction) for an introduction to the scripting language.

Come talk to us in our [Discord Server](https://discord.gg/39wmC2e) if you're interested or have questions!

## Mission Statement
When making gameplay systems of higher complexity, blueprint visual scripting can easily lead to unmaintainable spaghetti.
However, making these systems in C++ imposes long iteration times, and can be daunting for designers or gameplay scripters to use.

With this plugin you can write gameplay in a customized version of [Angelscript](https://www.angelcode.com/angelscript/), a simple but powerful scripting language.

Some key benefits that this plugin helps achieve:
* **Rapid Iteration** - Scripts can be reloaded instantly in the editor, letting developers focus on creating cool shit instead of waiting for compiles and editor restarts.
* **Improved Cooperation** - Because programmers and designers are no longer separated by the C++/Blueprint divide, they can work closely together using the same systems and tools.
* **Performance** - Angelscript performs significantly better than blueprint for game scripting, and approaches native C++ performance when using [transpiled scripts](/cpp-bindings/precompiled-data) in a shipping build.

## Features
### Familiar but Simplified

![](/img/scripting.png)

Programmers used to working in Unreal C++ will find the scripts instantly familiar, but with many key simplifications to make life easier for designers and avoid common C++ pitfalls.

### Script Hotreload for Fast Iteration
See your changes to scripted actors and components reflected immediately when you hit save.

Non-structural changes can be reloaded *while* playing your game for lightning-fast iteration.

![Properties](/img/properties.png)

### Scripting with Full Editor Support

To make scripting easier, a [Visual Studio Code Extension](https://marketplace.visualstudio.com/items?itemName=Hazelight.unreal-angelscript) is available implementing full Language Server Protocol support.

This includes support for many editor features, such as:
* Code Autocompletion
* Error Diagnostics
* Rename Symbol
* Find All References
* Semantic Highlighting

![](/img/timer.png)

### Integration with existing C++ and Blueprint workflows
Angelscript classes can override any BlueprintImplementableEvent you expose from C++,
and can be used seamlessly as base classes for child blueprints.

Use whatever combination of tools fits your workflow best.

![Functions](/img/functions.png)

### Debugging Support Through Visual Studio Code
Debug your script code through the Visual Studio Code extension.
Set breakpoints and inspect variables, and step through your scripts to find issues.

![Debugging](/img/debug.png)