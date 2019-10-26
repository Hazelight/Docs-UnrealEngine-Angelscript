Title: Angelscript Features

# Angelscript Features
### Overriding Functions
#### BlueprintNativeEvent / BlueprintImplementableEvent
Any `UFUNCTION()` declared as a blueprint event in C++ can be overridden by an angelscript subclass.

To do so, the function in angelscript must be declared as `UFUNCTION(BlueprintOverride)`, to indicate
that it wants to override a blueprint event.

The 'Receive', 'Received_', 'BP_' and 'K2_' prefixes are automatically stripped from event names as they are bound.
(For example, the C++ BlueprintImplementableEvent AActor::ReceiveBeginPlay should be overridden as just 'BeginPlay' in angelscript) 

#### Overriding Script Functions in Blueprint
Functions in angelscript can be declared as `UFUNCTION(BlueprintEvent)` to allow blueprint subclasses
of the angelscript class to override the function. This functions similarly to a BlueprintNativeEvent
in C++. You must always provide a base implementation in angelscript.

Example:

```
class AEventActor : AActor
{
	// Overrides the AActor::ReceiveTick() event declared BlueprintImplementableEvent in C++
	UFUNCTION(BlueprintOverride)
	void Tick(float DeltaSeconds)
	{
		// Call a function that might be overridden by a blueprint
		EnterBlueprint();
	}

	// Can be overridden by a blueprint that inherits from AEventActor
	UFUNCTION(BlueprintEvent)
	void EnterBlueprint()
	{
		Print("Blueprint did not override EnterBlueprint!");
	}
}
```

### Property Accessors
In angelscript, when accessing an `Object.Property`, it will automatically be rewritten
as a call to `Object.GetProperty()` if appropriate.

Similarly, attempting to set a property can automatically call the `SetProperty()` function
(provided it only takes a single argument).

This works both for functions exposed to angelscript from C++ as well as functions declared
in angelscript classes.

Example:

```
class APropertyAccessorActor : AActor
{
	UFUNCTION(BlueprintOverride)
	void BeginPlay()
	{
		// The following two are equivalent, and both call the C++ function SetActorLocation()
		ActorLocation = FVector(0.f, 100.f, 0.f);
		SetActorLocation(FVector(0.f, 100.f, 0.f));

		// Will automatically call the GetActorValue() accessor declared below
		Print(" Actor "+Name+" = "+ActorValue);
	}

	FString GetActorValue()
	{
		// Automatically calls the C++ GetActorLocation() accessor
		return ActorLocation.ToString();
	}
}
```

### FName Literals
A lot of unreal systems use `FName` to efficiently pass around arbitrary
names without having to copy and compare strings a lot. The name struct itself
is just an index into a name table, and creating an `FName` from a string does
a table lookup or inserts a new entry into the table.

A common pattern in C++ is to declare a global/static property for an `FName`
constant to use, so that the name table lookup only happens once at startup.

In angelscript, this pattern is simplified by using name literals.
Any string that is declared as `n"NameLiteral"` marks a name literal, and
will automatically be initialized at angelscript compile time, removing
the nametable lookup from runtime.

Name literals have many uses. An example of using a name literal to bind
a delegate to a `UFUNCTION()` in angelscript:

Example:

```
delegate void FExampleDelegate();

class ANameLiteralActor : AActor
{
	void UseNameLiteral()
	{
		FExampleDelegate Delegate;
		Delegate.BindUFunction(this, n"FunctionBoundToDelegate");
		Delegate.ExecuteIfBound();

		// Due to the name literal, no string manipulation happens
		// in calls to UseNameLiteral() during runtime.
	}

	UFUNCTION()
	void FunctionBoundToDelegate()
	{
		Print("Delegate executed");
	}
}
```

### Unreal Network
Unreal networking features are supported to a similar extent as they are in blueprint.

`UFUNCTION()`s can be marked as `NetMulticast`, `Client`, `Server` and/or `BlueprintAuthorityOnly`
in their specifiers, functioning much the same as they do in C++. The function body will
automatically be used as an RPC, whether calling it from angelscript or blueprint.

Unlike C++, angelscript RPC functions default to being reliable. If you want an unreliable
RPC message, put the `Unreliable` specifier in the `UFUNCTION()` declaration.

`UPROPERTY()`s can be marked as `Replicated`. Optionally, you can set a condition for 
their replication as well, similar to the dropdown for blueprint properties. This can be
done with the `ReplicationCondition` specifier.

Similar to C++ and Blueprint networking, in order for RPCs and replicated properties to
work, the actor and component need to be set to replicate. In angelscript this can
be done using `default` statements.

Example:

```
class AReplicatedActor : AActor
{
	// Set the actor's replicates property to default to true,
	// so its declared replicated properties work.
	default bReplicates = true;

	// Will always be replicated when it changes
	UPROPERTY(Replicated)
	bool bReplicatedBool = true;

	// Only replicates to the owner
	UPROPERTY(Replicated, ReplicationCondition = OwnerOnly)
	int ReplicatedInt = 0;

	// Calls OnRep_ReplicatedValue whenever it is replicated
	UPROPERTY(Replicated, ReplicatedUsing = OnRep_ReplicatedValue)
	int ReplicatedValue = 0;

	UFUNCTION()
	void OnRep_ReplicatedValue()
	{
		Print("Replicated Value has changed!");
	}
}
```

Available conditions for `ReplicationCondition` match the ELifetimeCondition enum in C++, and are as follows:

- None
- InitialOnly
- OwnerOnly
- SkipOwner
- SimulatedOnly
- AutonomousOnly
- SimulatedOrPhysics
- InitialOrOwner
- Custom
- ReplayOrOwner
- ReplayOnly
- SimulatedOnlyNoReplay
- SimulatedOrPhysicsNoReplay
- SkipReplay


It is also possible to specify `ReplicatedUsing` on a replicated `UPROPERTY` that will be called whenever
the value of that property is replicated. Note that any function used with `ReplicatedUsing` must be
declared as a `UFUNCTION()` so it is visible to unreal.