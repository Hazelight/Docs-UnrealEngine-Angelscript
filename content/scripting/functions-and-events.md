+++
title = "Functions and BlueprintEvents"
weight = 10
sort_by = "weight"
+++

# Functions and BlueprintEvents
## Plain Script Functions
Functions can be declared as methods in a class or globally.
By default any function you declare can be called from script but is not accessible to blueprint.

```cpp
class AExampleActor : AActor
{
	void MyMethod()
	{
		MyGlobalFunction(Actor);
	}
}

void MyGlobalFunction(AActor Actor)
{
	if (!Actor.IsHidden())
	{
		Actor.DestroyActor();
	}
}
```

## Functions that can be called from Blueprint
To make it so a function can be called from blueprint, add a `UFUNCTION()` specifier above it.

```cpp
class AExampleActor : AActor
{
	UFUNCTION()
	void MyMethodForBlueprint()
	{
		Print("I can be called from a blueprint!");
	}
}
```

> **Note:** Unlike in C++, it is not necessary to specify `BlueprintCallable`, it is assumed by default.

## Overriding BlueprintEvents from C++
To override a Blueprint Event declared from a C++ parent class, use the `BlueprintOverride` keyword.
You will use this often to override common events such as `BeginPlay` or `Tick`:

```cpp
class AExampleActor : AActor
{
	UFUNCTION(BlueprintOverride)
	void BeginPlay()
	{
		Print("I am a BeginPlay override");
	}

	UFUNCTION(BlueprintOverride)
	void Tick(float DeltaSeconds)
	{
		Print("I get called every tick");
	}
}
```

> **Note:** For C++ functions that don't explicitly specify a `ScriptName` meta tag, some name simplification is automatically done to remove common prefixes.  
> For example, the C++ event is called `ReceiveBeginPlay`, but the preceeding `Receive` is removed and it just becomes `BeginPlay` in script.  
> Other prefixes that are removed automatically are `BP_`, `K2_` and `Received_`.

## Overriding a Script Function from Blueprint
Often you will want to create a blueprint that inherits from a script parent class.
In order to make a function so it can be overridden from a child blueprint, add the `BlueprintEvent` specifier.

```cpp
class AExampleActor : AActor
{
	UFUNCTION(BlueprintEvent)
	void OverridableFunction()
	{
		Print("This will only print if not overridden from a child BP.");
	}
}
```

> **Note:** Script has no split between `BlueprintImplementableEvent` and `BlueprintNativeEvent` like C++ has.
> All script functions require a base implementation, although it can be left empty.

### Tip: Separate Blueprint Events
One pattern that is employed often in Unreal C++ and can be useful in script as well is to have separate script and blueprint events.
This way you can guarantee that the script code runs as well as any blueprint child code, this way you will never run into issues from a Designer forgetting to do "Add call to parent function".

For example, a pickup actor might do:
```cpp
class AExamplePickupActor : AActor
{
	void PickedUp()
	{
		// We always want this script code to run, even if our blueprint child wants to do something too
		Print(f"Pickup {this} was picked up!");
		SetActorHiddenInGame(false);

		// Call the separate blueprint event
		BP_PickedUp();
	}

	// Allows blueprints to add functionality, does not contain any code
	UFUNCTION(BlueprintEvent)
	void BP_PickedUp() {}
}
```

## Global Functions
Any script function in global scope can also have `UFUNCTION()` added to it.
It will then be available to be called from any blueprint like a static function.

This lets you create functions not bound to a class, similar to how Blueprint Function Libraries work.

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: Consolas, 'Courier New', monospace;font-weight: normal;font-size: 16px;line-height: 21px;white-space: pre;"><div><span style="color: #6a9955;">// Example global function that moves an actor somewhat</span></div><div><span style="color: #4fc1ff;">UFUNCTION</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">ExampleGlobalFunctionMoveActor</span><span style="color: #d4d4d4;">(</span><span style="color: #4ec9b0;">AActor</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">Actor</span><span style="color: #d4d4d4;">, </span><span style="color: #4ec9b0;">FVector</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">MoveAmount</span><span style="color: #d4d4d4;">)</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #9cdcfe;">Actor</span><span style="color: #d4d4d4;">.</span><span style="color: #9cdcfe;">ActorLocation</span><span style="color: #d4d4d4;"> += </span><span style="color: #9cdcfe;">MoveAmount</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">}</span></div></div>

![](/img/example-global-function.png)

> **Tip:** Comments above function declarations become tooltips in blueprint, just like in C++

## Calling Super Methods
When overriding a script function with another script function, you can use the same `Super::` syntax from Unreal C++ to call the parent function.
Note that script methods can be overridden without needing `BlueprintEvent` on the base function (all script methods are virtual). However when overriding a `BlueprintEvent`, you *will* need to specify `BlueprintOverride` on the overrides.

```cpp
class AScriptParentActor : AActor
{
	void PlainMethod(FVector Location)
	{
		Print("AScriptParentActor::PlainMethod()");
	}

	UFUNCTION(BlueprintEvent)
	void BlueprintEventMethod(int Value)
	{
		Print("AScriptParentActor::BlueprintEventMethod()");
	}
}

class AScriptChildActor : AScriptParentActor
{
	// Any script method can be overridden
	void PlainMethod(FVector Location) override
	{
		Super::PlainMethod();
		Print("AScriptChildActor::PlainMethod()");
	}

	// Overriding a parent BlueprintEvent requires BlueprintOverride
	UFUNCTION(BlueprintOverride)
	void BlueprintEventMethod(int Value)
	{
		Super::BlueprintEventMethod();
		Print("AScriptChildActor::BlueprintEventMethod()");
	}
}
```

> **Note:** When overriding a C++ `BlueprintNativeEvent`, it is not possible to call the C++ Super method due to a technical limitation. You can either prefer creating `BlueprintImplementEvent`s, or put the base implementation in a separate callable function.
