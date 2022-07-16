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

> **Note:** Unlike in C++, it is **not** necessary to declare a property as `UPROPERTY()` in order to avoid it being garbage collected. All object references in script are automatically inserted into the GC.

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">TeleportActorToOtherActor</span><span style="color: #d4d4d4;">(</span><span style="color: #4ec9b0;">AActor</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ActorReference</span><span style="color: #d4d4d4;">, </span><span style="color: #4ec9b0;">AActor</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">TeleportToActor</span><span style="color: #d4d4d4;">)</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4ec9b0;">FTransform</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">TeleportToTransform</span><span style="color: #d4d4d4;"> = </span><span style="color: #9cdcfe;">TeleportToActor</span><span style="color: #d4d4d4;">.</span><span style="color: #dcdcaa;">GetActorTransform</span><span style="color: #d4d4d4;">();</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #9cdcfe;">ActorReference</span><span style="color: #d4d4d4;">.</span><span style="color: #dcdcaa;">SetActorTransform</span><span style="color: #d4d4d4;">(</span><span style="color: #9cdcfe;">TeleportToTransform</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #d4d4d4;">}</span></div></div>

## Default Accessibility for Properties
`UPROPERTY()` variables are `EditAnywhere` and `BlueprintReadWrite` by default. This can be overridden by specifying `NotBlueprintCallable` or `NotEditable`.

The default access specifiers for properties in script can be configured from Project Settings.

The intent of this is to simplify property specifiers. Since `UPROPERTY()` is not needed for correct garbage collection, you should only specify it when you want it to be accessible in the editor/blueprint.

## Use the `default` Keyword Instead of Constructors
Instead of using object constructors, which can run at unpredictable times during hot reloads, any default values for properties should be specified in the class body.

For setting values on subobjects, use the `default` keyword:

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #569cd6;">class</span><span style="color: #d4d4d4;"> </span><span style="color: #4ec9b0;">AExampleActor</span><span style="color: #d4d4d4;"> : </span><span style="color: #4ec9b0;">AActor</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// Set default values for class properties in the class body</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UPROPERTY</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">float</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ConfigurableValue</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">5.0</span><span style="color: #d4d4d4;">;</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #6a9955;">// Set default values for subobjects with `default` statements</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UPROPERTY</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">DefaultComponent</span><span style="color: #d4d4d4;">)</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4ec9b0;">UCapsuleComponent</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">CapsuleComponent</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">default</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">CapsuleComponent</span><span style="color: #d4d4d4;">.</span><span style="color: #9cdcfe;">CapsuleHalfHeight</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">88.0</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">default</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">CapsuleComponent</span><span style="color: #d4d4d4;">.</span><span style="color: #9cdcfe;">CapsuleRadius</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">40.0</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">default</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">CapsuleComponent</span><span style="color: #d4d4d4;">.</span><span style="color: #9cdcfe;">bGenerateOverlapEvents</span><span style="color: #d4d4d4;"> = </span><span style="color: #569cd6;">true</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">}</span></div></div>

## Floating Point Width
With Unreal 5.0, Epic has started using `double`s for all gameplay-related vectors, rotators, etc.
Rather than confuse people that are used to working with `float` in blueprint, they decided to keep calling these doubles `float` everywhere in the editor like before.

The angelscript integration follows this decision, meaning that when you declare a `float` in script, it is actually a 64-bit double value.
To create a floating-point variable with a specific width, you can explicitly use the `float32` or `float64` types.

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #569cd6;">float</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ValueDouble</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">1.0</span><span style="color: #d4d4d4;">; </span><span style="color: #6a9955;">// &lt;-- This is a 64-bit double-precision float</span></div><div><span style="color: #569cd6;">float32</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ValueSingle</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">1.f</span><span style="color: #d4d4d4;">; </span><span style="color: #6a9955;">// &lt;-- This is a 32-bit single-precision float</span></div><div><span style="color: #569cd6;">float64</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">ValueAlsoDouble</span><span style="color: #d4d4d4;"> = </span><span style="color: #b5cea8;">1.0</span><span style="color: #d4d4d4;">; </span><span style="color: #6a9955;">// &lt;-- This is *also* a 64-bit double-precision float</span></div></div>