Title: C++ Usage and Bindings

### UFUNCTIONs and UPROPERTYs
Any UFUNCTION() that is declared BlueprintCallable/BlueprintPure is automatically bound to
be callable in angelscript. UFUNCTION()s can be declared with the new specifier 'ScriptCallable'
to allow them to be called from angelscript without being exposed to blueprint.

Any UPROPERTY() that is declared BlueprintReadOnly/BlueprintReadWrite or has a
any Edit flag such as EditDefaults/EditAnywhere, is automatically exposed to
angelscript. UPROPERTY()s in C++ can be declared with the new specifiers
'ScriptReadOnly' and 'ScriptReadWrite' to allow them to be accessed from
angelscript without being exposed to blueprint.

The K2_ and BP_ prefixes are automatically stripped from function names when they are bound.

### Static Namespace Simplification
Static UFUNCTION()s are exposed and callable as well. The name of the namespace is simplified
for nicer looking script code using a few simple rules.

For example, the function 'UKismetSystemLibrary::GetGameName()' is simplified to just 'System::GetGameName()' in script.

If the name of the class matches any of these patterns, it will be simplified:
* U...Statics
* U...Library
* U...FunctionLibrary
* UKismet...Library
* UKismet...FunctionLibrary
* UBlueprint...Library
* UBlueprint...FunctionLibrary

### Loading Order
The angelscript plugin will load after your game's module by default. This allows classes declared
in your C++ game module to be detected and usable in script.

If you need to use or access angelscript from your game module for whatever reason (by adding the 'AngelscriptCode'
module to your PulicDependencyModuleNames for example), make sure to call FAngelscriptCodeModule::InitializeAngelscript()
from your game module's StartupModule() manually, so the loading happens in the correct place.

### Garbage Collection
Everything in angelscript is fully integrated with the unreal garbage collector.

References to UObjects are automatically added to the GC, even if you do not mark them as UPROPERTY() in script, unlike in C++.

### Overview of Differences for C++ UE4 Developers
* Variables of UObject types are automatically references. Pointers do not exist, all methods/properties are accessed using dot syntax.
* UPROPERTY() variables are EditAnywhere and BlueprintReadWrite by default. This can be overridden by specifying NotBlueprintCallable or NotEditable.
* Use the 'default' keyword (see Example_MovingObject.as) to override property default values in subobjects. Do not use constructors as they interfere with hot reloading.
* Properties, even UObject or AActor references, will be correctly garbage collected even without adding them as UPROPERTY().