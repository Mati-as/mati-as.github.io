---
layout: single
title:  "About LINQ"
categories: "shader"
tag: [C#]
toc: true
sidebar:
    nav: "counts"
---


#LINQ to XML

## is LINQ that powerful?
Back to my college, i didn't know much about LINQ, neither C#. 
However, as soon as i started my career as a game programmer,LINQ was always on the go.(luckily, i was taking in charge of another part. another FYI, sometimes, a good programmer just implements what they need without it.) 

in this post. i'd like to simple explanation about How LINQ is so powerful in C# and give more details about LINQ when it comes to incorporating XML.


## So.. what is LINQ?
LINQ stands for 'Language Intergrated Query' that is introduced from C# 3.0. Above all, not only for XML, but also tons of data can be handled in a **'standardized'** way. Let's cut to the point right away.

```
using System;
using System.Collections.Generic;
using System.Linq;

...

var names = new List<string>
    "Seoul", "Otawa", "Paris", "London","Hong Kong","Tokyo"
    };

IEnumerable<string> query = names.Where(s=>s.Length <=5>);
foreach(string s in query) Console.WriteLine(s)

```

in this code, **Where** is used to find the elements that satisfies the condition that its Length should be the same or less than 5. To understand the powerful usage of LINQ, let's compare it with the type,'List'.

```
Array.FindAll(arrayOfName, s=>s.Length <=5>);
```

with **FindAll** Method, we can also filter out some elements in just the same way as **Where** method.
However, using different APIs that's up to specific method can burden the implementing process. or maintenance would be crazily long if when what to use for data structures is undecided. 