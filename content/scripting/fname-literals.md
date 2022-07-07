+++
title = "FName Literals"
weight = 50
sort_by = "weight"
+++

# FName Literals
A lot of unreal systems use `FName` to efficiently pass around arbitrary
names without having to copy and compare strings a lot. The name struct itself
is just an index into a name table, and creating an `FName` from a string does
a table lookup or inserts a new entry into the table.

A common pattern in C++ is to declare a global/static variable for an `FName`
constant to use, so that the name table lookup only happens once at startup.

In angelscript, this pattern is simplified by using name literals.
Any string that is declared as `n"NameLiteral"` will be
initialized at angelscript compile time, removing
the nametable lookup from runtime.

Name literals have many uses. An example of using a name literal to bind
a delegate to a `UFUNCTION()` in angelscript:

<div class="code_block" style="color: #d4d4d4;background-color: #1e1e1e;font-family: Consolas, 'Courier New', monospace;font-weight: normal;font-size: 16px;line-height: 21px;white-space: pre;"><div><span style="color: #569cd6;">delegate</span><span style="color: #d4d4d4;"> </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #4ec9b0;">FExampleDelegate</span><span style="color: #d4d4d4;">();</span></div><br><div><span style="color: #569cd6;">class</span><span style="color: #d4d4d4;"> </span><span style="color: #4ec9b0;">ANameLiteralActor</span><span style="color: #d4d4d4;"> : </span><span style="color: #4ec9b0;">AActor</span></div><div><span style="color: #d4d4d4;">{</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4ec9b0;">TMap</span><span style="color: #d4d4d4;">&lt;</span><span style="color: #4ec9b0;">FName</span><span style="color: #d4d4d4;">, </span><span style="color: #569cd6;">int</span><span style="color: #d4d4d4;">&gt; </span><span style="color: #9cdcfe;">ValuesByName</span><span style="color: #d4d4d4;">;</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">UseNameLiteral</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; {</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #4ec9b0;">FName</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">NameVariable</span><span style="color: #d4d4d4;"> = n</span><span style="color: #ce9178;">"MyName"</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #9cdcfe;">ValuesByName</span><span style="color: #d4d4d4;">.</span><span style="color: #dcdcaa;">Add</span><span style="color: #d4d4d4;">(</span><span style="color: #9cdcfe;">NameVariable</span><span style="color: #d4d4d4;">, </span><span style="color: #b5cea8;">1</span><span style="color: #d4d4d4;">);</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #4ec9b0;">FExampleDelegate</span><span style="color: #d4d4d4;"> </span><span style="color: #9cdcfe;">Delegate</span><span style="color: #d4d4d4;">;</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #9cdcfe;">Delegate</span><span style="color: #d4d4d4;">.</span><span style="color: #dcdcaa;">BindUFunction</span><span style="color: #d4d4d4;">(</span><span style="color: #569cd6;">this</span><span style="color: #d4d4d4;">, n</span><span style="color: #ce9178;">"FunctionBoundToDelegate"</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #9cdcfe;">Delegate</span><span style="color: #d4d4d4;">.</span><span style="color: #dcdcaa;">ExecuteIfBound</span><span style="color: #d4d4d4;">();</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #6a9955;">// Due to the name literal, no string manipulation happens</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #6a9955;">// in calls to UseNameLiteral() during runtime.</span></div><div><span style="color: #d4d4d4;">&#160; &#160; }</span></div><br><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #4fc1ff;">UFUNCTION</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; </span><span style="color: #569cd6;">void</span><span style="color: #d4d4d4;"> </span><span style="color: #dcdcaa;">FunctionBoundToDelegate</span><span style="color: #d4d4d4;">()</span></div><div><span style="color: #d4d4d4;">&#160; &#160; {</span></div><div><span style="color: #d4d4d4;">&#160; &#160; &#160; &#160; </span><span style="color: #dcdcaa;">Print</span><span style="color: #d4d4d4;">(</span><span style="color: #ce9178;">"Delegate executed"</span><span style="color: #d4d4d4;">);</span></div><div><span style="color: #d4d4d4;">&#160; &#160; }</span></div><div><span style="color: #d4d4d4;">}</span></div></div>