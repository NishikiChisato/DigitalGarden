---
tags:
  - CPP
  - Professional-CPP
---
# The auto keyword
The auto keyword has a number of different uses:
## To deduce a function’s return type
We can specify the return type of a function as `auto` as long as all return paths both have the same return type:

```cpp
auto func(int val) {
	if (val > 0) {
		return 1;
	} else if (val == 0) {
		return 0;
	}
	return -1;
}
```

In this example, there are three return paths, and both return `int`, therefore we can use `auto` keyword for return type inference.

## To define structured bindings

Noted that structured bindings only works with auto, instead of explicitly specifying type:

```cpp
std::pair<int, std::string> func1() {
	return {0, ""};
}

std::pair<int, int> func2() {
	return {1, 1};
}

auto [l1, r1] = func1();
int [l2, r2] = func2(); // error
```


## To deduce the type of an expression
`auto` can be used to let compiler to automatically deduce the type of variable at compile time, which is much useful for complicated type:

```cpp
auto x{123}; // expression '123' has type 'int', so 'x' has tpye 'int'
auto y{func()}; // variable 'y' would get the return type of 'func' function
```

Noted that using `auto` to deduce the type of an expression **always strips** away reference and `const` qualifiers. That's say:

```cpp
const std::string str{"Hotaru"};
const std::string& func(const std::string& str) {return str;}

auto s1{str}; // 's1' has type std::string
auto s2{func()}; // 's2' has type std::string
auto s3{std::as_const(str);} // 's3' has type std::string
```

### The auto& syntax
If we want to specify reference or `conse` qualifiers, we have to manually specify it:

```cpp
const auto s4{str}; // 's4' has type const std::string
const auto& s5{func()}; // 's5' has type const std::string&
```

> [!tip] Always keep in mind that auto strips away reference and const qualifiers and thus creates a copy! If you do not want a copy, use auto& or const auto&.

### The auto* syntax
When we use `auto` to deduce a pointer type, it's recommend to use `auto*` instead of `auto`:

```cpp
int* ptr{nullptr};
auto p1{ptr}; // ok, bad
auto* p2{ptr}; // ok, good
```

This is because when we need to apply `const` qualifiers to such variable, `const auto` isn't doing what we expect it to do!

```cpp
const auto p3{ptr}; // deduce to int* const
```

But, if we use `const auto*`, we can run out of such problem:

```cpp
const auto* p4{ptr}; // deduce to const int*
```

The `const` qualifiers has two levels: top-level const and low-level const. `const auto` applies `const` to variable itself. When variable is pointer, `const auto` results in const pointer. But when we suffix asterisk after `auto`, we have a way to specify whether `const` is top-level or low-level.

### Copy list initialization and Direct list initialization
There are two types of initialization using braced initialized list:
- Direct list: `T obj{arg1, arg2, ...};`
- Copy list: `T obj = {arg1, arg2, ...};`

The difference between them is that, the latter have to do one extra copy assignment comparing with the former.

In combination with `auto`, there is an important difference between:

```cpp
auto obj1{1}; // ok, obj1 is type int with value 1
auto obj2{1,2}; // error, too many elements
auto obj3 = {1}; // ok, obj3 is type std::initializer_list<int> with value {1}
auto obj4 = {1,2}; // ok, obj4 is type std::initializer_list<int> with value {1,2}
auto obj5 = {1,1.1}; // error, all elements should both have the same type
```


## To deduce the type of non-type template parameters




## To define abbreviated function templates




## To use with `decltype(auto)`




## To write functions using the alternative function syntax




## To write generic lambda expressions



# The decltype keyword
The `decltype` takes an expression as argument and computes the type of that expression. Besides, the main difference between `auto` and `decltype` is that `decltype` doesn't strip reference and `const` qualifiers, as shown here:

```cpp
const std::string& func(const std::string& val) {return val;}
int x{1};

decltype(x) y{10}; // 'y' has type int with value 10
decltype(func) z{func("Hotaru")}; // 'z' has type const std::string&
```

Noted that `decltype` doesn't evaluate the expression, compiler computers the type of that expression.