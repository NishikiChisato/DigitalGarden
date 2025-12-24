---
tags:
  - book/Professional-CPP
  - CPP
  - cpp/keyword
  - status/seedlings
aliases:
  - deduce a function's type
---
# to deduce a function's return type
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