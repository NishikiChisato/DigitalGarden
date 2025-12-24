---
tags:
  - book/Professional-CPP
  - CPP
  - status/seedlings
aliases:
  - argument index
  - argument indices
---
We can either omit the index from all replacement fields or specify,for **all replacement fields**, the zero-based index of one of the value passed to `std::format/std::print/std::println` as the second and subsequent arguments. We are allowed to specify the certain index multiple times if we want to output this value multiple times. If the index is omitted, the values passed as second and subsequent arguments are used in their given order for all replacement fields. As the following example shown:

```cpp
int val1{1};
int val2{2};

std::println("{} {}", val1, val2);
std::println("{0} {1}", val1, val2);
std::println("{1} {0}", val1, val2);
std::println("{0} {0}", val1);
std::println("{0} {}", val1, val2);
```

The first four work correctly, but the fifth one won't, since we should omit or specify the index for **all replacement fields**, we can specify the half of them.