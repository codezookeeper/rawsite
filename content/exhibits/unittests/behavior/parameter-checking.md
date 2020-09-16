---
title: "Test multiple paramters one at a time"
date: 2020-09-12T01:45:38-04:00
tags: ["C-sharp","Unit Tests","Unit Tests - Behavior"]
---

{{< figure src="https://i.dailymail.co.uk/1s/2019/01/18/09/8685802-6606291-image-a-52_1547804638791.jpg" caption="Parametercheckus Oneatatimeus" >}}

## Classification
> Class: [Unit test]  
> Order: [Behavior]  
> Family: [Too much]  
> Genus: [Parametercheckus]  
> Species: [Oneatatimeus]  

## Related
"Python version"[python-parameter-checking]  
"Typescript version"[typescript-parameter-checking]  

## Wild sample

Code file
```c#
public static bool HasRareProperty(Animal animal, AnimalTrait trait)
{
	if (trait is null)
	{
		throw new ArgumentNullException(nameof(trait));
	}

	// insert useful code here...

	return false; // nevermind, just return
}
```

Unit test
```c#
public static void Given_NullTrait_Should_ThrowArgumentNullException()
{
	Assert.Throws<ArgumentNullException>(
		() => HasRareProperty(null, null)
	);
}
```

## Wild observations

This code seems simple enough. The test will verify the ArgumentNullException as intended. It even uses `nameof`[^1] well! The issue here is subtle, we are not testing behavior, we are testing implementation. The problem could best be demonstrated with a simple hypothetical: what if we also decide to test `animal` for null in the future? Would this test still catch the null check for `trait`? Or would it be possible that it's just catching the null check for `animal`?!

[^1]: `nameof` prevents the parameter name and a string litteral "coincidentally" having the same name. see [Nameofius Myvariable]

## Tamed code

Unit test
```c#
public static void Given_NullTrait_Should_ThrowArgumentNullException()
{
	Assert.Throws<ArgumentNullException>(
		() => HasRareProperty(new Cow(), null)
	);
}
```

## Domesticated observations

Even though we know the current implementation doesn't have any checks for `animal`, we pass a complete `animal` anyway because our tests should not be written with intimate knowledge of the implementation. They should be written to _only_ test the behavior[^2] we are looking for.

[^2]: for more information see [Behavior based testing]

## Related exhibits

[something]  
[something else]  