---
tags:
  - book/Professional-CPP
  - CPP
  - status/evergreen
aliases:
  - high-level conversion
---
# High-level Conversion

High-level conversion is pretty straightforward, since the C++ library handles most of the work for you. All of the following function both defined in `<string>` header.

## Converting to string
For converting to string from numeric value, we simply use the following function:

```cpp
std::string to_string(T val);
```

This function is available to convert numeric value into `std::string`, where `T` can be `(unsigned) int, (unsigned) long, (unsigned) long long, float, double, or long double`. All of these functions create and return a new `std::string` object and manage all necessary memory allocation for you.

## Converting from string
For converting to numeric value from string, we simply use the family of following functions:

```cpp
int stoi(const std::string& str, size_t *pos = nullptr, int base = 10);
long stol(const std::string& str, size_t *pos = nullptr, int base = 10);
unsigned long stoul(const std::string& str, size_t *pos = nullptr, int base = 10);
long long stoll(const std::string& str, size_t *pos = nullptr, int base = 10);
unsigned long long stoull(const std::string& str, size_t *pos = nullptr, int base = 10);
float stof(const std::string& str, size_t *pos = nullptr);
double stod(const std::string& str, size_t *pos = nullptr);
long double stold(const std::string& str, size_t *pos = nullptr);
```

In these prototypes, `str` is the `std::string` that we want to convert, `pos` is a pointer that receives the index of the first unconverted character, and `base` is the mathematical base that used in conversion. The `pos` pointer can be `nullptr`, in which case it's ignored. These function handle the leading white-space, and throw `std::invalid_argument` or `std::out_of_range` if no conversion can be performed or the converted value is outside the range of the return type, respectively.

By default, the `base` for these functions both are `10`, which assumes the string can be converted to decimal number. We can also specify it to another number. Note that if we specify it to `0`, in which case it would automatically figure out the base of the given number as the follows:
- If the number starts with `0x` or `0X`, it's interpreted as hexadecimal number.
- If the number starts with `0`, it's interpreted as octal number.
- Otherwise, it's interpreted as decimal number.
