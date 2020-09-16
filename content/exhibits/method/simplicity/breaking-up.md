---
title: "Breaking Up"
date: 2020-09-16T19:04:19-04:00
tags: ["C-sharp", ]
draft: true
---

{{< figure src="https://mysciencesquad.weebly.com/uploads/1/1/8/3/118361154/495600766-612x612_orig.jpg" caption="Oversized method breaking into smaller pieces" >}}

## Classification
> Class: [Method](../../index)  
> Order: [Simplicity](../index)  
> Family: Length  
> Genus: Code  
> Species: Fungus  

## Related
"Python version"[]  
"Typescript version"[]  

## Wild sample

Code file
```c#
// too long and horrible to display on a family friendly site
```

## Wild observations

Wild samples of this condition abound. In wild production code, methods have been found well in excess of _2000_ lines! These methods often develop a cancerous property called copy/paste/modify where they are replicated and then go through further mutations resulting in a code base that has many common, but difficult to extract, components.

## Wild disease

Occasionally it is recognized that long methods are bad, but without a viable remidy, developers will occasionally resort to breaking the larger method's sections into new methods and replacing the larger method with calls to those methods. Although this achieves the aimiable goal of making the method smaller, it actually enourages method rot because the logic is now disjointed and optimizations are even harder to find.[^1]

[^1]: In general, new methods should not be refactored out of old methods unless they [logically-DRY] the codebase.

## Domesticated observations

The best way to tame code of this type comes from a 3 step treatment:
1. Inline any one-time use methods to reduce swelling from wild method rot cited above.
2. Eliminate local variables that modify state or depend on a fixed execution order as much as possible.
3. Look for repeated structures[^2] and not for repeated logic. When the code structures are extracted, the logic will remain in a purer state.[^3]

[^2]: See [iteration-evolution] for examples of DRYing repeated structures.

[^3]: See [safari/logical-DRY] for real world examples of split rot and logical DRYing.

## Related exhibits

[something]  
[something else]  

**[Comments](https://github.com/codezookeeper/rawsite/blob/master/content/exhibits/bool-smell.md)**  
**[Edits/PR](https://github.com/codezookeeper/rawsite/blob/master/content/exhibits/bool-smell.md)**