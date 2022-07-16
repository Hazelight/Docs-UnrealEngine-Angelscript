+++
title = "Editor-Only Script"
weight = 110
sort_by = "weight"
+++

# Editor-Only Script
Some properties, functions, or classes from C++ are only available in the editor.
If you try to use them in a cooked game, the script will fail to compile.

This could be things like actor labels, editor subsystems or visualizers, etc.

## Preprocessor Blocks
If you need to use editor-only code within a class, you can use the `#if EDITOR` preprocessor statement around the code.
Any code within these blocks is not compiled outside of the editor, and can safely use editor-only functionality.

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #569cd6;">class</span><span style="color: #d4d4d4;"> </span><span style="color: #4ec9b0;">AExampleActor</span><span style="color: #d4d4d4;"> : </span><span style="color: #4ec9b0;">AActor</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #569cd6;">#if EDITOR</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">default</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">PivotOffset</span><span style="color: #d4d4d4;"> = </span><span style="color: #4ec9b0;">FVector</span><span style="color: #d4d4d4;">(</span><span style="color: #b5cea8;">0</span><span style="color: #d4d4d4;">, </span><span style="color: #b5cea8;">0</span><span style="color: #d4d4d4;">, </span><span style="color: #b5cea8;">10</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #569cd6;">#endif</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UFUNCTION</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">BlueprintOverride</span><span style="color: #d4d4d4;">)</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">ConstructionScript</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; {</span></div><div><span style="color: #569cd6;">#if EDITOR</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #dcdcaa;">SetActorLabel</span><span style="color: #d4d4d4;">(</span><span style="color: #ce9178;">"Example Actor Label"</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #569cd6;">#endif</span></div><div><span style="color: #d4d4d4;">&#160; &#160; }</span></div><div><span style="color: #d4d4d4;">}</span></div></div>

> **Tip:** Other useful macro conditions:  
> `EDITORONLY_DATA` - Whether properties that are only relevant to the editor are readable.  
> `RELEASE` - Whether the game is built in either the Shipping or Test build configurations.  
> `TEST` - Whether the game is built in Debug, Development, or Test build configurations.

## Editor-Only Directories
It is also possible for complete scipt files to be skipped outside of the editor.
Any folder named `Editor` will be completely ignored by the script compiler in cooked builds.
This can be useful to put for example editor visualizer or subsystem classes under an `Editor` folder.

In addition to the `Editor` folder, the two other folder names `Examples` and `Dev` are also ignored in cooked builds.

# Testing with Simulate-Cooked Mode
Because of editor-only scripts, it's possible to have scripts in your project that work and compile in the editor, but will fail once the game is cooked.
To make it easier to detect these errors - for instance in a CI task - you can use the `-as-simulate-cooked` command line parameter.

When simulate cooked mode is active, editor-only properties and classes are not available in script, and `#if EDITOR` blocks are compiled out.

You can use this in combination with the `AngelscriptTest` commandlet to make sure everything compiles.
An unreal command line to test whether the scripts compile might look like:
```sh
UnrealEditor-Cmd.exe "MyProject.uproject" -as-simulate-cooked -run=AngelscriptTest
```