+++
title = "Differences with Unreal C++"
weight = 300
sort_by = "weight"
+++

# Overview of Differences for C++ Unreal Developers
While the script files will feel familiar to developers used to working in C++ with Unreal, there are a number of differences.
Most of the differences are intended to simplify the script language for people coming in from using Blueprint.

Some differences you will most likely run into are highlighted here.

## Objects Instead of Pointers
Any variable declared with a `UObject` type is automatically an object reference. Pointers do not exist in the script language.
This is similar to how object reference variables work in blueprint.
There is no `->` arrow operator in script, everything happens with `.` dots.

> **Note:** Unlike in C++, it is _not_ necessary to declare a property as `UPROPERTY()` in order to avoid it being garbage collected. All object references in script are automatically inserted into the GC.

```cpp
void TeleportActorToOtherActor(AActor ActorReference, AActor TeleportToActor)
{
	FTransform TeleportToTransform = TeleportToActor.GetActorTransform();
	ActorReference.SetActorTransform(TeleportToTransform);
}
```

## Default Accessibility for Properties
`UPROPERTY()` variables are `EditAnywhere` and `BlueprintReadWrite` by default. This can be overridden by specifying `NotBlueprintCallable` or `NotEditable`.

The default access specifiers for properties in script can be configured from Project Settings.

The intent of this is to simplify property specifiers. Since `UPROPERTY()` is not needed for correct garbage collection, you should only specify it when you want it to be accessible in the editor/blueprint.

## Use the `default` Keyword Instead of Constructors
Instead of using object constructors, which can run at unpredictable times during hot reloads, any default values for properties should be specified in the class body.

For setting values on subobjects, use the `default` keyword:

```cpp
class AExampleActor : AActor
{
	// Set default values for class properties in the class body
	UPROPERTY()
	float ConfigurableValue = 5.0;

	// Set default values for subobjects with `default` statements
	UPROPERTY(DefaultComponent)
	UCapsuleComponent CapsuleComponent;
	default CapsuleComponent.CapsuleHalfHeight = 88.0;
	default CapsuleComponent.CapsuleRadius = 40.0;
	default CapsuleComponent.bGenerateOverlapEvents = true;
}
```

## Floating Point Width
With Unreal 5.0, Epic has started using `double`s for all gameplay-related vectors, rotators, etc.
Rather than confuse people that are used to working with `float` in blueprint, they decided to keep calling these doubles `float` everywhere in the editor like before.

The angelscript integration follows this decision, meaning that when you declare a `float` in script, it is actually a 64-bit double value.
To create a floating-point variable with a specific width, you can explicitly use the `float32` or `float64` types.

```cpp
float ValueDouble = 1.0; // <-- This is a 64-bit double-precision float
float32 ValueSingle = 1.f; // <-- This is a 32-bit single-precision float
float64 ValueAlsoDouble = 1.0; // <-- This is *also* a 64-bit double-precision float
```