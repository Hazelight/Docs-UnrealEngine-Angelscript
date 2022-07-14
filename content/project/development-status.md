+++
title = "Development Status"
weight = 10
+++

# Development Status
At [Hazelight](http://hazelight.se), this project's angelscript integration
has been actively used for game development since early 2018. It is being used daily by 30+ developers on our team.

Our previous game [It Takes Two](https://www.ea.com/games/it-takes-two) was shipped with the majority of its gameplay written in angelscript.
The released game contains over 700,000 lines of angelscript code, written in more than 5,000 script files.

Hazelight does not guarantee any maintenance or support beyond what
is already provided on this site. If you are considering using the
angelscript integration in a commercial project, we highly recommend
having an engine programmer available to investigate issues that may occur
in your usage of it.


## Platform Support
The plugin is regularly used and tested at Hazelight on the Windows, PS5, and Xbox Series X|S platforms.
Other studios have succesfully built and ran it on other platforms not tested internally by Hazelight.

If you have any questions about other platforms, feel free to [join our discord server](https://discord.gg/39wmC2) and ask around.


## Known Limitations
* `Super::Function()` calls only work for parent functions in angelscript. It is not possible to call the C++ super function when overriding a BlueprintNativeEvent.
* Unreal Interfaces (IInterface/UInterface) are not supported for use in angelscript.