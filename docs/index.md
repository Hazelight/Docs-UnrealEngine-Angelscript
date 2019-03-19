# Unreal Engine Angelscript
## What is it?
UnrealEngine-Angelscript is a set of engine modifications and a plugin for UE4 that integrates a
full-featured scripting language. It is actively developed and used by [Hazelight](http://hazelight.se),
creators of [A Way Out](https://www.ea.com/games/a-way-out).

See [Getting Started](getting-started/) for an introduction to the scripting language.

## Features
### Immediate Hot Reload of script files
See your changes to scripted actors and components reflected immediately when you hit save.

Non-structural changes can even be reloaded *while* playing your game for lightning-fast iteration.

### Debugging support through Visual Studio Code
Debug your script code through the Visual Studio Code extension.
Set breakpoints and inspect variables.

### Seamless integration with existing C++ and Blueprint workflows
Angelscript classes can override any BlueprintImplementableEvent you expose from C++,
and can be used seamlessly as base classes for child blueprints.

Use whatever combination of tools fits your workflow best.