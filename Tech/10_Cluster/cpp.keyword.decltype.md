---
tags:
  - book/Professional-CPP
  - CPP
  - cpp/keyword
  - status/seedlings
---
The `decltype` takes an expression as argument and computes the type of that expression. Besides, the main difference between `auto` and `decltype` is that `decltype` doesn't strip reference and `const` qualifiers, as shown here:

```cpp
const std::string& func(const std::string& val) {return val;}
int x{1};

decltype(x) y{10}; // 'y' has type int with value 10
decltype(func) z{func("Hotaru")}; // 'z' has type const std::string&
```

Note that `decltype` doesn't evaluate the expression, compiler computers the type of that expression.