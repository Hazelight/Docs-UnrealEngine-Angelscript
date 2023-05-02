+++
title = "GameplayTags"
weight = 105
sort_by = "weight"
+++

Gameplay Tags are used in many unreal systems. See the [Unreal Documentation on Gameplay Tags](https://docs.unrealengine.com/5.1/en-US/using-gameplay-tags-in-unreal-engine/) for more details.

During the X* phase all defined** `FGameplayTag` will be automatically bound to the global object `GameplayTags`, with all non-alphanumeric characters (including the dot separators) turned into underscore `_`.

// Assuming there is a GameplayTag named "UI.Action.Escape"
FGameplayTag EscapeAction = GameplayTags::UI_Action_Escape;

*=  lifecyle events. when is this? I only got the hyphen error during our packaged builds and not locally
**= is there a limitation? does it have to be in an .ini file?