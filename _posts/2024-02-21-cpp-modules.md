---
title: "Notes on modules in C++20"
categories: 
  - Software
tags:
  - software
  - projects
  - c++
---

Brief set of notes on modules in case it saves anyone else some searching.

---

Notes from Feb 21...

Modules are a feature introduced in C++20. They serve as an alternative to header files and precompiled headers. See this [Microsoft Learn post for more.](https://learn.microsoft.com/en-us/cpp/build/compare-inclusion-methods?view=msvc-170) 

Right now, modules work nicest for Windows users. There's a bit of extra legwork to get modules working on UNIX systems that I've only found so far in SO posts. (ChatGPT isn't super helpful as a Google replacement here because it only goes up to November 2022, meaning it lacks training data on C++23 and some advnaces made by various compilers in adopting the C++20 standard.) [This post in particular was quite helpful.](https://stackoverflow.com/questions/69452740/shared-libraries-and-c20-modules)

I haven't figured out Intellisense for modules and honestly don't want to bother.

For more, try searching in `r/cpp` or looking at examples here. I want to use modules for my custom engine and will report back at some point with how smoothly they work and how big the savings are on link time. They will likely be low considering the scope of the engine, but it'd be nice to see...
