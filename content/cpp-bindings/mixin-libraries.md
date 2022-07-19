+++
title = "Script Mixin Libraries"
weight = 10
+++

# Script Mixin Libraries
Instead of adding new namespaced static functions for scripts, it can be useful to provide additional _methods_ on existing types.

To do this, use the `ScriptMixin` metadata on a C++ class with static functions.  
Any static function whose first argument matches the type specified in the metadata will be bound as a method on that type.

A common use case for this is to add methods to `USTRUCT`s, which cannot have `UFUNCTION`s on them, and as such cannot have methods using normal automatic bindings.

## Mixin Libraries for Structs
For example, the following C++ mixin library class adds two new methods to the `FVector` struct in script:

```cpp
UCLASS(Meta = (ScriptMixin = "FVector"))
class UFVectorScriptMixinLibrary : public UObject
{
	GENERATED_BODY()
public:

	// This will be accessible in script as
	//     FVector Vector;
	//     Vector.ResetTo(4.0);
	UFUNCTION(ScriptCallable)
	static void ResetTo(FVector& Vector, float NewValue)
	{
		Vector = FVector(NewValue, NewValue, NewValue);
	}

	// This will become a const method, as it takes
	// a const reference to the mixin type:
	//  Usable in script as both   Vector.SummedValue
	//                        or   Vector.GetSummedValue()
	UFUNCTION(ScriptCallable)
	static float GetSummedValue(const FVector& Vector)
	{
		return Vector.X+Vector.Y+Vector.Z;
	}
}
```

## Mixin Libraries for Classes
It is also possible to add new methods to `UCLASS`es. In that case, take a pointer to the type as the first argument.

The following C++ mixin library adds a new method to all `AActor`s in script:

```cpp
UCLASS(Meta = (ScriptMixin = "AActor"))
class UMyActorMixinLibrary : public UObject
{
	GENERATED_BODY()
public:

	// This can be used in script as:
	//  Actor.TeleportToOrigin();
	UFUNCTION(ScriptCallable)
	static void TeleportToOrigin(AActor* Actor)
	{
		Actor->SetActorLocation(FVector(0, 0, 0));
	}
}
```

> **Note:** The angelscript plugin comes with a number of mixin libraries in it, mostly to expose C++ functionality that is not normally exposed to blueprint.  
> For example, see the [TimelineComponentMixinLibrary](https://github.com/Hazelight/UnrealEngine-Angelscript/blob/angelscript-master/Engine/Plugins/Angelscript/Source/AngelscriptCode/Public/FunctionLibraries/TimelineComponentMixinLibrary.h)
