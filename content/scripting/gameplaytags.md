+++
title = "GameplayTags"
weight = 105
sort_by = "weight"
+++

Gameplay Tags are used in many unreal systems. See the [Unreal Documentation on Gameplay Tags](https://docs.unrealengine.com/5.1/en-US/using-gameplay-tags-in-unreal-engine/) for more details.

All `FGameplayTag` will automatically be bound to the global object `GameplayTags`. All non-alphanumeric characters, including the dot separators, are turned into underscore `_`.

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: 'Terminus (TTF) for Windows', Consolas, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 19px;white-space: pre;"><div><span style="color: #6a9955;">// Assuming there is a GameplayTag named "UI.Action.Escape"</span></div><div><span style="color: #4ec9b0;">FGameplayTag</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">TheTag</span><span style="color: #d4d4d4;"> = <span style="color: #4ec9b0">GameplayTags</span></span><span style="color: #d4d4d4;">::</span><span style="color: #9cdcfe;">UI_Action_Escape<span style="color: #d4d4d4">;</span></span></div></div>