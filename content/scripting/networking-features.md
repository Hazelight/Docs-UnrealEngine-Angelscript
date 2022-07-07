+++
title = "Networking Features"
weight = 80
sort_by = "weight"
+++

# Unreal Networking Features
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

```cpp
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