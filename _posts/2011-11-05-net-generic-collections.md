---
layout: post
title: ".Net Generic Collections"
description: ""
category: posts
tags: [.net]
---
{% include JB/setup %}

In 2.0 of the framework a slew of generic collections were added, in this post I wanted to walk through some of the common types and talk about the differences between them. Prior to 2.0 you were pretty limited to un-typed collections like ArrayList. and HashTable, and if you wanted a strongly typed collection you needed to create your own by inheriting from CollectionBase or implementing one of the other interfaces that were suitable for building your own collection class. Fortunately we now have the System.Collections.Generic namespace which gives us many more options. You could simply use List&lt;T&gt; for all of your collection needs and call it a day, but what about all those other classes and interfaces? Let’s dig a little deeper…

### List&lt;T&gt; and IList&lt;T&gt;

Since List&lt;T&gt; is the largest and probably the most common let’s start there. This class implements the following interfaces: IList&lt;T&gt;, ICollection&lt;T&gt;, IEnumerable&lt;T&gt;, IList, ICollection, IEnumerable.

These three interfaces drive the functionality of this class (along with most of the .net collection classes), as you can see it implements the generic and non-generic versions. The non-generic ones give us the non-type specific methods such as Clear and Count (they also give us the other methods but they are marked as protected). The generic interfaces make up most of the rest of the class with methods such as Add, Remove, Insert, etc. These all take in a generic type of T so that they are type safe.

One of the things that sets List&lt;T&gt; apart is IList&lt;T&gt;. What this gives us is an indexer along with the three methods IndexOf, InsertAt, and RemoveAt (which all require the index). On top of these extra interface methods List&lt;T&gt; also gives a bunch of additional methods of it’s own such as Exists, Find, ForEach, BinarySearch and many many more. These additional methods should play a big part in your decision to use IList&lt;T&gt; of List&lt;T&gt;. Using the interface gives you the flexibility of using any of the classes that implement IList&lt;T&gt; (arrays are a good example), but you lose the extra functionality from List&lt;T&gt;.

### ICollection&lt;T&gt;

Next up is the interface ICollection&lt;T&gt;. We already know that this guy is implemented by List&lt;T&gt;, but some of the other classes that utitlize it are Dictionary&lt;TKey,TValue&gt;, LinkedList&lt;T&gt;, and various other classes and interfaces. What ICollection&lt;T&gt; brings to the party are the methods Add(T), Clear(), Contains(T), CopyTo(T[],Int32), Remove(T) and the properties Count and IsReadOnly. As you can see, ICollection&lt;T&gt; gives you pretty much everything you need to manage a collection of strongly typed objects

### IEnumerable&lt;T&gt;

Last but certainly not least is IEnumerable&lt;T&gt;. This one is no-doubtedly implemented by more classes and interfaces than the previous two. If you are working with a type that has a &lt;T&gt; at the end and you can iterate over it with a foreach loop, then it implements this interface (any non generic collections will implement IEnumerable). It contains one and only one method, GetEnumerator()  which returns IEnumerable&lt;T&gt;.

### Now What?

So now that we have a little background on these collections, how can and how should we use them? When I am need of a collection in my code I ask myself a few questions:
1.Do I know the definite size for my collection?
2.Do I need to modify the collection?
3.Do I need to access the collection by index?

If you answered yes to #1 and no to the rest then just use an array:

{% gist 6131986 %}

If you answered no to all 3 then I would go with IEnumerable&lt;T&gt;. Since all you are mostly going to do is access items in a foreach loop or in a Linq expression this makes the most sense:

{% gist 6132191 %}

If you answered no to #1, yes to #2, and no to #3 then go with ICollection&lt;T&gt;. You are going to want to be able to add and remove items and possibly need the size, but not still just iterate over it with foreach or Linq:

{% gist 6132206 %}

Finally, if you said no to #1 and yes to #2 and #3 then go with IList&lt;T&gt; or List&lt;T&gt;. This gives you the most flexibility since they both implement ICollection&lt;T&gt; which implements IEnumerable&lt;T&gt;. 

{% gist 6132216 %}

### Conclusion

So why not always use List&lt;T&gt; then? This usually comes down to memory footprint. Since the more functionality you have the bigger the  memory allocation is going to be. I usually find myself either using IList&lt;T&gt; or IEnumerable&lt;T&gt; the most, but hopefully this article gave you some background to help you make an educated decision.