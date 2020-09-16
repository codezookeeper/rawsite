---
title: "Boolean Literal Smell"
date: 2020-09-12T20:06:38-04:00
tags: ["C-sharp", ]
---

{{< figure src="https://upload.wikimedia.org/wikipedia/commons/5/57/Striped_Skunk.jpg" caption="Booleanus Smellicus - pictured here: 'true' and 'false'" >}}

## Classification
> Class: [Method]  
> Order: [Simplicity]  
> Family: [Conditional]  
> Genus: [Booleanus]  
> Species: [Smellicus]  

## Related
"Python version"[]  
"Typescript version"[]  

## Wild sample

Code file
```c#
public static bool NeedsToBeUpdated(Article article)
{
	var needsUpdate = false;

	if (article.PublishDate < DateTime.Now.AddDays(-100) && article.CurrentEvent)
	{
		needsUpdate = true;
	}

	return needsUpdate;
}
```

## Wild observations

Many times in the wild you can find someone using an `if` statement to check a condition and then set a boolean. It doesn't matter if the boolean is set to `true` or `false`, this is a code smell. The condition itself is the answer to the question and makes for a much more expressive statement.

If the code appears like above but with `true` and `false` switched, the if condition can be inverted with a logical not.

If the logic appears complicated and still uses `true` and `false`, look for a way to express pieces of the expression as a truth table[^1].

[^1]: see [truth-table]

## Tamed code

Unit test
```c#
public static bool NeedsToBeUpdated(Article article) =>
	article.PublishDate < DateTime.Now.AddDays(-100) 
	&& article.CurrentEvent;
```

## Domesticated observations

When the code is reduced to the condition, the logic is much clearer and reusability becomes more apparent[^2].

[^2]: see [DRY-is-an-API]


## Related exhibits

[something]  
[something else]  