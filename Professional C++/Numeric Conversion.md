C++ library provides both high-level and low-level conversion for numeric conversion. We can use them to convert fixed/floating point to string and vice versa.

# High-level Conversion

High-level conversion is pretty straightforward, since the C++ library handles most of the work for you. All of the following function both defined in `<string>` header.

For converting to string from numeric value, we simply use the following function:

```cpp
std::string to_string(T val);
```

This function is available to convert numeric value into `std::string`, where `T` can be `(unsigned) int, (unsigned) long, (unsigned) long long, float, double, or long double`. All of these functions create and return a new `std::string` object and manage all necessary memory allocation for you.

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


# Low-level Conversion

Different from the high-level conversion functions, low-level conversion function only has two prototypes, all defined in `<charconv>`. These function don't automatically handle memory allocation and don't work directly with `std::string`, but instead they work with the raw buffer provided by the user. Since the nature of these, the performance of low-level conversion functions can be orders of magnitude faster than high-level conversion functions. These functions are also designed for perfect round-tripping, which means that serializing a numerical value to a string representation followed by deserializing the resulting string back to a numerical value results in the exact same value as the original one. Besides for high performance and perfect round-tripping, these function also provides [[locale independent]] conversions, so we can use these functions to serialize/deserialize numerical data from/to human-readable data format(JSON/XML and so on).

For converting to string from integer value(floating-point is later), we simply use the following function:

```cpp
to_char_result to_chars(char* first, char* last, IntegerT value, int base = 10);
```

Here, `IntegerT` can be any signed or unsigned integer type or `char`. The result is of type `to_char_result`, which is defined as follows:

```cpp
struct to_chars_result { char* ptr; errc ec; };
```

The `ptr` member is either equal to the one-past-the-end pointer of the written characters if the conversion is successful, in which case the `ec` member equal to the default constructed `errc`, or equal to `last` if the conversion failed, in which case the `ec` member equal to `errc::value_too_large`.

>[!INFO] Here is the examples:

Similarly, the following set of functions are available for floating-point numeric value:

```cpp
to_chars_result to_chars(char* first, char* last, FloatT value);
to_chars_result to_chars(char* first, char* last, FloatT value, chars_format format);
to_chars_result to_chars(char* first, char* last, FloatT value, chars_format format, int precision);
```




