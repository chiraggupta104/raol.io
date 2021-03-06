+++
date = "2014-11-06"
title = "Foldr in C#"
tags = ["programming", "functional"]
+++

I'm exploring and trying to learn Haskell, or at least get better understanding of 
functional programming. So I thought it would be interesting to tease the brain
and reimplement some of Haskell parts in my "mother tongue" C#.

<!--more-->

So as it is clear from the title, I'm going to implement `foldr` function. In general
fold is a higher order function that does processing of list and return result. We can 
think of it as of reduce function. `foldl` differs from `foldr` only in order of processing
lists. From left to right or from right to left.

So let's looks at it declaration

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f z [] = z
foldr f z (x:xs) = f x (foldr f z xs)
```

So let's reformulate problem in C# terms. So foldr is a method, which accepts 
some `Func<>`, parameter to return, when list is empty, and the list itself.
Written in code it looks like:

```csharp
TResult Foldr<TArg, TResult>(Func<TArg, TResult, TResult> func, TResult b, IEnumerable<TArg> en)
{
    if(!en.Any())
    {
        return b;
    }
    
    return func(en.First(), Foldr(func, b, en.Skip(1)));
}
```

Not as elegant as if we had pattern matching in C# but still looks quite good.
And now we can use our `foldr` to calculate sum of a list:

```csharp
// When list is empty its sum is 0.
Foldr((a,b) => a + b, 0, Enumerable.Range(0, 10))
```