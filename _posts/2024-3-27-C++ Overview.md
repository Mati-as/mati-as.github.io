---
layout: single
title:  "Diving Into Modern C++"
categories: "C++"
tag: [C++]
toc: true
sidebar:
    nav: "counts"
---

-Disclaimer:  This post is about Personal Study Journey. The following post is a casual dive into the world of Modern C++ programming,
 based on my personal study notes and explorations. It's meant to share insights and reflections on what I've learned so far.
 Please keep in mind that while I've done my best to ensure accuracy, there might be some inaccuracies or misunderstandings.
This is all part of the learning process, and I welcome any corrections or discussions in the comments below. 
 Happy reading!"


### The Popularity and Appeal of C++
C++ ranks high among frequently used languages, right up there with Python, Java, C#, and C.
C++ is in demand globally, in a wide range of fields, for several reasons:
Firstly,developers have full control over performance aspects. This level of control is unmatched by other programming languages, allowing developers to manage nearly all performance-related aspects directly.
C++ enables the use of low-level languages (like assembly or kernel operations) without the need for additional low-level languages for memory management or hardware manipulation. Yet, you can do almost everything within C++ itself.
Beyond just low-level language capabilities, C++ also incorporates object-oriented programming features, offering the best of both worlds.
Its versatility makes C++ suitable for a wide array of devices, from embedded devices to supercomputers.
With over 40 years of updates and improvements, C++ has developed numerous methodologies and development strategies to effectively solve various software problems. This also facilitates collaboration among developers and enhances the utility of the language itself.
Application Areas
C++ is used in numerous industries, including operating systems, compilers, artificial intelligence, image editing, web browsers, high-performance computing, embedded systems, scientific computing, databases, video games, finance, NASA's Webb Telescope, and the Perseverance Mars mission, among others.

### The Philosophy of C++
C++ prioritizes performance without sacrificing it, adhering to the zero overhead principle or zero-cost abstraction. In essence, it means that writing code at an abstract level shouldn’t incur more overhead than low-level code.
It enforces compile-time safety as much as possible, allowing for maximum error and effect analysis during the compile phase.
The language offers a powerful and flexible user-defined type system.
It provides a programming model that allows complete control to the developer under modularization and certain constraints.
C++ is designed to use minimal memory and energy, making it suitable for environments with limited resources and hardware platforms.
Its characteristics are highly compatible with static analysis (the process of identifying potential errors and bugs without running the code) and safe software, contributing to its portability.

### Weaknesses of C++
Due to its compatibility with C, C++ inherits many of C’s unstable defaults and structures, leading to security issues and unpredictable behavior, such as buffer overflows.
Defects discovered through user experience and testing can be hard to fix, potentially impacting software stability.
C++ has become increasingly complex, often prioritizing code clarity and readability at the expense of productivity and expressiveness.

### Alternatives to C++
C++ Epochs: Strives to maintain backward compatibility while evolving.
Carbon Language: Offers less complexity but is safer.
Circle C++ Compiler: Focuses on efficiency and code productivity.
Cppfront: Aims to be simpler and safer.