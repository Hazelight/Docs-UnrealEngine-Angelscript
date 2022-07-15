+++
title = "Function Libraries"
weight = 30
sort_by = "weight"
+++

# Function Libraries
Interacting with unreal from scripts often happens through function libraries.
These are exposed to script as namespaces containing a set of related functions.

For example, to set a timer you can call `System::SetTimer()`:

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #569cd6;">class</span><span style="color: #d4d4d4;"> </span><span style="color: #4ec9b0;">ATimerExample</span><span style="color: #d4d4d4;"> : </span><span style="color: #4ec9b0;">AActor</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UFUNCTION</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">BlueprintOverride</span><span style="color: #d4d4d4;">)</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">BeginPlay</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; {</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #6a9955;">// Execute this.OnTimerExecuted() after 2 seconds</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #4ec9b0;">System</span><span style="color: #d4d4d4;">::</span><span style="color: #dcdcaa;">SetTimer</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">this</span><span style="color: #d4d4d4;">, n</span><span style="color: #ce9178;">"OnTimerExecuted"</span><span style="color: #d4d4d4;">, </span><span style="color: #b5cea8;">2.0</span><span style="color: #d4d4d4;">, bLooping = </span><span style="color: #569cd6;">false</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #d4d4d4;">&#160; &#160; }</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UFUNCTION</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">private</span><span style="color: #d4d4d4;"> </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">OnTimerExecuted</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; {</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #dcdcaa;">Print</span><span style="color: #d4d4d4;">(</span><span style="color: #ce9178;">"Timer executed!"</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #d4d4d4;">&#160; &#160; }</span></div><div><span style="color: #d4d4d4;">}</span></div></div>

## Common Libraries
Clicking the library will bring you to the API documentation for them, listing the available functions:

* [Math::](/api/index.html#CClass:Math) - All standard math functionality
* [Gameplay::](/api/index.html#CClass:Gameplay) - Game functionality such as streaming, damage, player handling
* [System::](/api/index.html#CClass:System) - Engine functionality such as timers, traces, debug rendering
* [Niagara::](/api/index.html#CClass:Niagara) - Spawning and controlling particle systems
* [Widget::](/api/index.html#CClass:Widget) - UMG widget functionality

## Namespace Simplification
The functions for function libraries in script are automatically taken from blueprint function library classes in C++.

Before binding, the angelscript plugin simplifies the name of the class to make a shorter namespace.
For example, the functions in the `System::` namespace are automatically sourced from the `UKismetSystemLibrary` class in C++.

Common prefixes and suffix that get stripped automatically are:
* U...Statics
* U...Library
* U...FunctionLibrary
* UKismet...Library
* UKismet...FunctionLibrary
* UBlueprint...Library
* UBlueprint...FunctionLibrary

For some examples of how namespaces are simplified:
* `UNiagaraFunctionLibrary` becomes `Niagara::`
* `UWidgetBlueprintLibrary` becomes `Widget::`
* `UKismetSystemLibrary` becomes `System::`
* `UGameplayStatics` becomes `Gameplay::`

# Math Library
Because blueprint and C++ have fairly different ways of doing math code, we have decided to keep the `Math::` namespace in script closer to the C++ `FMath::` namespace in general.

Sticking closer to C++ math eases the transition for experienced programmers and lets code be ported between the two more easily.

This means that `UKismetMathLibrary` gets ignored for automatic binding.