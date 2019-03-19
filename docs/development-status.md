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
Running angelscript in a packaged/cooked build is currently functional, provided the 
`ProjectName/Script/` folder is staged or copied into the resulting build.

However, there are several areas in which performance improvements will have to be
made in the future. These include the script compile time at engine startup as well
as script runtime performance.

For context, at the time of this writing we are running about 100,000 lines of
angelscript code in our project. Compiling it all takes roughly 5-6 seconds on
engine startup. We have found angelscript to vastly outperform equivalent blueprint
script at runtime, but improvements are still possible.

Plans have been made for this, but are unlikely to be implemented until later in our game's development
cycle, when they become necessary. No guarantees on when or what.

### Console Support
We have preliminary builds running with angelscript on PS4 and Xbox One consoles,
but we will not be doing a significant amount of testing on these until later.

Start up time performance improvements will be extra-critical for shipping on consoles,
as they are significantly slower than PCs overall.

## Known Issues
* Changing a 'DefaultComponent' to a different class without changing the name of the variable can cause it to become uneditable in derived blueprints.
  Change the name of the component property when changing the class to work around this.
* Super::Function() calls only work for parent functions in angelscript. It is not possible to call the C++ super function when overriding a BlueprintNativeEvent.
* Unreal Interfaces (IInterface/UInterface) are not supported for use in angelscript at this time.