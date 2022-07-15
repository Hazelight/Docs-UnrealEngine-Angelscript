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

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #569cd6;">class</span><span style="color: #d4d4d4;"> </span><span style="color: #4ec9b0;">AReplicatedActor</span><span style="color: #d4d4d4;"> : </span><span style="color: #4ec9b0;">AActor</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// Set the actor's replicates property to default to true,</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// so its declared replicated properties work.</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">default</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">bReplicates</span><span style="color: #d4d4d4;"> = </span><span style="color: #569cd6;">true</span><span style="color: #d4d4d4;">;</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// Will always be replicated when it changes</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UPROPERTY</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">Replicated</span><span style="color: #d4d4d4;">)</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">bool</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">bReplicatedBool</span><span style="color: #d4d4d4;"> = </span><span style="color: #569cd6;">true</span><span style="color: #d4d4d4;">;</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// Only replicates to the owner</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UPROPERTY</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">Replicated</span><span style="color: #d4d4d4;">, </span><span style="color: #569cd6;">ReplicationCondition</span><span style="color: #d4d4d4;"> = OwnerOnly)</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">int</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ReplicatedInt</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">0</span><span style="color: #d4d4d4;">;</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// Calls OnRep_ReplicatedValue whenever it is replicated</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UPROPERTY</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">Replicated</span><span style="color: #d4d4d4;">, </span><span style="color: #569cd6;">ReplicatedUsing</span><span style="color: #d4d4d4;"> = OnRep_ReplicatedValue)</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">int</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ReplicatedValue</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">0</span><span style="color: #d4d4d4;">;</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UFUNCTION</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">OnRep_ReplicatedValue</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; {</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #dcdcaa;">Print</span><span style="color: #d4d4d4;">(</span><span style="color: #ce9178;">"Replicated Value has changed!"</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #d4d4d4;">&#160; &#160; }</span></div><div><span style="color: #d4d4d4;">}</span></div></div>

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