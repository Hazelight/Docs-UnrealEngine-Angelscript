# Development Status
At [Hazelight](http://hazelight.se), this project's angelscript integration
has been actively used for game development since early 2018. It is used daily by a
 team of 20+ programmers and designers.

While definitely holding up for our development use, the project will be considered in
a **beta** state at least until we have sucessfully shipped our game with it.
Primary areas of improvement to consider it production ready are listed below.

Hazelight does not guarantee any maintenance or support beyond what
is already provided on this site. If you are considering using the
angelscript integration in a commercial project, we highly recommend
having an engine programmer to investigate issues that may occur
in your usage of it.


## Status and Future Plans
### Packaged Builds and Performance
Running angelscript in a packaged/cooked build is currently fully functional, provided the 
`ProjectName/Script/` folder is staged or copied into the resulting build.

See the [Using Precompiled Data](/using-precompiled-data) page for steps to take
to improve startup and execution performance in packaged builds.

For context, at the time of this writing we have more than 300,000 lines of
angelscript code in our project at hazelight. Compiling it all on engine startup
without using precompiled data takes roughly 10-15 seconds on Windows. Using
precompiled data this is reduced to around 5 seconds of startup time.

We have found angelscript to vastly outperform equivalent blueprint
script at runtime. When using precompiled data to create generated C++,
runtime performance approaches that of native C++ with a small overhead when traversing
back and forth with the blueprint virtual machine.

### Platform Support
Aside from Windows, the angelscript plugin has been succesfully built for and is regularly tested on Xbox One and PS4.
There are currently no plans to extend this to any other platforms, but if you are interested in working on supporting
one yourself, feel free to [join the discord server](https://discord.gg/39wmC2).


## Known Issues
* Super::Function() calls only work for parent functions in angelscript. It is not possible to call the C++ super function when overriding a BlueprintNativeEvent.
* Unreal Interfaces (IInterface/UInterface) are not supported for use in angelscript at this time.