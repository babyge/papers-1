# Effective Modern C++

## Introduction

If you're an experienced C\++ programmer and are anything like me, you initially approached C\++ 11 thinking, "Yes, yes, I get it. It's C++, only more so.".

## Chapter 1 Deducing Types
C\++ 98 had a single set of rules fro type deduction: the one for function templates. C\++11 modifies that ruleset a bit and adds two more, one for auto and one for decltype. C\++ 14 then extends the usage contexts in which auto and decltype may be employed. The increasingly widespread application of type deduction frees you from the tyranny of spelling out types that are obvious or redundant. It makes C\++ software more adaptable, because changing a type at one point in the source code automatically propagates through type deduction to other locations. However, it can render code more difficult to reason about, because the types deduced by compilers may not be as apparent as you'd like.
Without a solid understanding of how type deduction operates, effective programming in modern C\++ is all but impossible. There are just too many contexts where type deduction takes place: in calls to function template, in most situations where auto appears, in decltype expressions, and, as of C\++ 14, where the enigmatic decltype(auto) construct is employed.
This chapter provides the information about type deduction that every C\++ developer requires. It explains how template type deduction works, how auto builds on that, and how decltype goes its own way. It even explains how you can force compilers to make the results of their type deductions visible, thus enabling you to ensure that compilers are deducting the types you want them to.

### Item 1: Understand template type deduction

When users of a complex system are ignorant of how it works, yet happy with what it does, that says a lot of about the design of the system. By this measure, template type deduction in C\++ is a tremendous success. Millions of programmers have passed arguments to template functions with completely satisfactory result, even though many of those programmers would be hard-pressed to give more than the haziest description of how the types used by those functions were deducted.
If that group includes you, I have good news and bad news. The good news is that type deduction for templates is the basis for one of modern C\++'s most compelling features: auto. If you were happy with how C\++98 deducted types for templates, you're set up to be happy with how C\++11 deduces type for auto. The bad news is that when the template type deduction rules are applied in the context of auto, they sometimes seem less intuitive than when they're applied to templates. For that reason, it's important to truly understand the aspects of template type deduction that auto builds on. This item covers what you need to know.

If you're willing to overlook a pinch of pseudocode, we can think of a function template as looking like this:
```cpp
template<typename T>
void f(ParamType param);
```
A call can look like this:
```cpp
f(expr); //call f with some expression
```
During complilation, compilers use expr to deduce two types: one for T and one for ParamType. These types are frequently different, because ParamType often contains adornments, e.g., const or reference qualifiers. For exmaple, if the template is declared this,
```cpp
template<typename T>
void f(const T& param); // ParamType is const T&
```
and we have this call,
```cpp
int x = 0;
f(x); // call f with an int
```
T is deduced to be int, but ParamType is deduced to be const int&.

Things to Remember
* During tmplate type deduction, arguments that are references are treated

### Item 2: Understand auto type deduction

### Item 3: Understand decltype

### Item 4: Know how to view deduced types

## Chapter 2 auto

### Item 5: Prefer auto to explicit type declarations

### Item 6: Use the explicitly typed initializer idiom when auto deduces undesired types

## Chapter 3 Moving to Modern C++

When it comes to big-name features, C\++11 and C\++14 have a lot of boast of. auto, smart pointers, move semantics, lambdas, concurrency - each is so important, I devote a chapter to it. It's essential to master thoes features, but becoming an effective modern C\++ programmer requires a series of smaller steps, too. Each step answers specific questions that arise during the journey from C\++98 to modern C\++. When should you use braces instead of of parentheses for object creation? Why are alias declarations better than typedefs? How does constexpr differ from const? What's the relationship between const member functions and thread safety? The list goes on and on. And one by one, this chapter provides the answers.

### Item 7: Distinguish between () and {} when creating objects

Depending on your perspective, syntax choices for object initialization in C\++11 embody either an embarrassment of riches or a confusing mess. As a general rule, initialization values may be specified with parentheses, an equals sign, or braces:
```cpp
int x(0); // initializer is in parentheses
int y = 0; // initializer follows "="
int z{0}; // initializer is in braces
```

In many cases, it's also possible to use an equals sign and braces together:
```cpp
int z = {0}; // initializer uses "=" and braces
```
For the remainder of this Item, I'll generally ignore the equals-sign-plus-braces syntax, because C++ usually treats it the same as the braces-only version.
The "confusing mess" lobby points out that the use of an equals sign for initialization often misleads C++ newbies into thinking that an assignment is taking place, even though it's not. For build-in types like int, the difference is academic, but for user-defined types, it's important to distinguish initialization from assignment, because different function calls are involved:
```cpp
Widget w1; // call default constructor
Widget w2 = w1; // not an assignment; call copy constructor
w1 = w2; // an assignment; calls copy operator=
```
Even with several initialization syntaxes, there were some situations where C\++98 had no way to express a desired initialization. For example, it wasn't possible to directly indicate that an STL container should be created holding a particular set of values(e.g., 1,3, and 5).
To address the confusion of multiple initialization syntaxes, as well as the fact that they don't cover all initialization scenarios, C\++11 introduces uniform initialization: a single initialization syntax that can, at least in concept, be used anywhere and express everything. It's based on braces, and for that reason I prefer the term braced initialization. "Uniform initialization" is an idea. "Braced initialization" is a syntactic construct.


### Item 8: Prefer nullptr to 0 and NULL

### Item 9: Prefer alias declarations to typedefs

### Item 10: Prefer scoped enums to unscoped enums

### Item 11: Prefer deleted functions to private undefined ones

### Item 12: Declare overriding functions override

### Item 13: Prefer const_iterators to iterators

### Item 14: Declare functions noexcept if they won't emit exceptions

### Item 15: Use constexpr whenever possible

### Item 16: Make const member functions thread safe

### Item 17: Understand special member function generation

## Chapter 4 Smart Pointers

### Item 18: Use std::unique_ptr for exclusive-ownership resource management

### Item 19: Use std::shared_ptr for shared-ownership resource management

### Item 20: Use std::weak_ptr for std::shared_ptr-like pointers that can dangle

### Item 21: Prefer std::make_unique and std::make_shared to direct use of new

### Item 22: When using the Pimpl Idiom, define special member functions in the implementation file

## Chapter 5 Rvalue References, Move Semantics, and Perfect Forwarding

### Item 23: Understand std::move and std::forward

### Item 24: Distinguish universal references from rvalue references

### Item 25: Use std::move on rvalue references, std::forward on universal references

### Item 26: Avoid overloading on universal references

### Item 27: Familiarize yourself with alternatives to overloading on universal references

### Item 28: Understand reference collapsing

### Item 29: Assume that move operations are not present, not cheap, and not used

### Item 30: Familiarize yourself with perfect forwarding failure cases

## Chapter 6 Lambda Expressions

### Item 31: Avoid default capture modes

### Item 32: Use init capture to move objects into closures

### Item 33: Use decltype on auto&& parameters to std::forward them

### Item 34: Prefer lambdas to std::bind

Chapter 7 The Concurrency API

### Item 35: Prefer task-based programming to thread-based

### Item 36: Specify std::launch::async if asynchronicity is essential

### Item 37: Make std::threads unjoinable on all paths

### Item 38: Be aware of varying thread handle destructor behaviour

### Item 39: Consider void futures for one-shot event communication

### Item 40: Use std::atomic for concurrency, volatile for special memory

Chapter 8 Tweaks

### Item 41: Consider pass by value for copyable parameters that are cheap to move and always copied

### Item 42: Consider emplacement instead of insertion






