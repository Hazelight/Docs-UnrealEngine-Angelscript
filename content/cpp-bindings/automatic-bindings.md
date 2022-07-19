+++
title = "Automatic Bindings"
weight = 10
+++

# Automatic Bindings
When the engine starts, the angelscript plugin automatically goes through all of unreal's reflection data.

Relevant types, properties and functions from C++ are automatically given bindings into Angelscript so they can be used from your scripts.

The general principle of automatic bindings is:  
If it can be used from Blueprint, it should be usable from Angelscript.

## Class Bindings
Classes in C++ that are marked with `UCLASS()` are automatically bound either when they have the `BlueprintType` specifier, or if they contain any functions with `BlueprintCallable`.

Classes can be skipped for automatic bindings by adding the `NotInAngelscript` metadata.

## Struct Bindings
Structs in C++ that are marked with `USTRUCT()` are automatically bound either when they have the `BlueprintType` specifier, or if they contain any properties that are blueprint-accessible or editable.

Structs can be skipped for automatic bindings by adding the `NoAutoAngelscriptBind` metadata.

## Property Bindings
### Read/Write Flags
C++ `UPROPERTY`s that are declared with `BlueprintReadWrite` or `BlueprintReadOnly` are automatically bound to script.

If the property is `BlueprintReadOnly`, it will become `const` and unable to be changed from script.

To expose a property to Angelscript without exposing it to blueprint, you can use the `ScriptReadWrite` or `ScriptReadOnly` specifiers.

### Editable Flags
Properties that are declared with any of the editable flags (`EditAnywhere`, `EditInstanceOnly` or `EditDefaultsOnly`) are also exposed to script.

> **Note:** If a property has an editable flag, but not a blueprint access flag, it will **only** be accessible in script from inside a class `default` statement.
> *See [Default Statements](/scripting/actors-components/#default-statements).*

### Skipping Properties
Properties can be skipped for angelscript binds even if they are otherwise accessible to blueprint by adding the `NotInAngelscript` metadata.

## Function Bindings
### Callable Flags
Any C++ `UFUNCTION` that has `BlueprintCallable` or `BlueprintPure` is automatically bound to script.

To expose a function to Angelscript without exposing it to blueprint, you can use the `ScriptCallable` specifier.

### Blueprint Events
`UFUNCTION`s with the `BlueprintImplementableEvent` and `BlueprintNativeEvent` specifiers can be overridden from script as well as blueprint.

> See [Overriding BlueprintEvents from C++](/scripting/functions-and-events/#overriding-blueprintevents-from-c)

### Skipping Functions
Functions can be skipped for angelscript binds even if they are otherwise accessible to blueprint by adding the `NotInAngelscript` metadata.

### Deprecated Functions
Functions marked as deprecated are not bound to script at all.

There is no deprecation warning functionality in script, so engine upgrades may necessitate script changes when Epic deprecates certain APIs.

## Static Functions
Static functions declared on `UCLASS`es are bound as namespaced global functions in script.

Note that for static functions only, the name of the class will go through [Namespace Simplification](/scripting/function-libraries/#namespace-simplification) when they are bound.

## Enum Bindings
Any `UENUM()` declared in C++ is automatically usable in script.