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


### The auto* syntax



## To deduce the type of non-type template parameters




## To define abbreviated function templates




## To use with `decltype(auto)`




## To write functions using the alternative function syntax




## To write generic lambda expressions

