---
tags:
  - book/Professional-CPP
  - CPP
  - cpp/keyword
  - status/seedlings
aliases:
  - define structured bindings
---
We can use auto to define structured bindings, but note that structured bindings only works with `auto`, instead of explicitly specifying type:

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

The variable declared and defined by structured bindings cannot reuse again, as the follows shown:

```cpp
auto [l, r] = func1();
auto [l, r] = func1(); // error
[l, r] = func1();      // error
```

