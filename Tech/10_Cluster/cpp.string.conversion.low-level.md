---
tags:
  - book/Professional-CPP
  - CPP
  - status/evergreen
aliases:
  - low-level conversion
---
# Low-level Conversion

Different from the high-level conversion functions, low-level conversion function only has two prototypes, all defined in `<charconv>`. These function don't automatically handle memory allocation and don't work directly with `std::string`, but instead they work with the raw buffer provided by the user. Since the nature of these, the performance of low-level conversion functions can be orders of magnitude faster than high-level conversion functions. These functions are also designed for perfect round-tripping, which means that serializing a numerical value to a string representation followed by deserializing the resulting string back to a numerical value results in the exact same value as the original one. Besides for high performance and perfect round-tripping, these function also provides [[locale independent]] conversions, so we can use these functions to serialize/deserialize numerical data from/to human-readable data format(JSON/XML and so on).

## Converting to string
For converting to string from integer value(floating-point is later), we simply use the following function:

```cpp
to_char_result to_chars(char* first, char* last, IntegerT value, int base = 10);
```

Here, `IntegerT` can be any signed or unsigned integer type or `char`. The result is of type `std::to_char_result`, which is defined as follows:

```cpp
struct to_chars_result { char* ptr; errc ec; };
```

The `ptr` member is either equal to the one-past-the-end pointer of the written characters if the conversion is successful, in which case the `ec` member equal to the default constructed `errc`, or equal to `last` if the conversion failed, in which case the `ec` member equal to `std::errc::value_too_large`.

Similarly, the following set of functions are available for floating-point numeric value:

```cpp
std::to_chars_result to_chars(char* first, char* last, FloatT value);
std::to_chars_result to_chars(char* first, char* last, FloatT value, std::chars_format format);
std::to_chars_result to_chars(char* first, char* last, FloatT value, std::chars_format format, int precision);
```

Here, `FloatT` can be any floating-point type: `float/double/long double`. Formatting can be specified with a combination of `chars_format` flags:

```cpp
enum class chars_format {
	scientific, // Style: (-)d.ddde±dd
	fixed, // Style: (-)ddd.ddd
	hex, // Style: (-)h.hhhp±d (Note: no 0x!)
	general = fixed | scientific
};
```

The default format is `std::chars_format::general`, which causes `std::to_chars` to convert the floating-point type to the decimal notation with the style of $(-)ddd.ddd$ or to the decimal exponent notation with the style of $(-)d.ddde\pm dd$, whichever results in the shortest representation with at least one digit before the decimal point(if present). If a format but no precision is specified, the precision is automatically determined to result in the shortest possible representation for the given format, with a maximum precision of six digits.

The following example demonstrate both integer and float-point type numeric conversion:

```cpp
int main() {
  constexpr size_t BufferSize{128};
  std::string out(BufferSize, ' ');
  const auto [ptr, err]{std::to_chars(out.data(), out.data() + out.size(), 12345)};
  if (err == std::errc{}) {
    std::println("Succ: {}", out);
  } else {
    std::println("Err: {}", ptr);
  }

  out.assign(BufferSize, ' ');

  const auto result =
      std::to_chars(out.data(), out.data() + out.size(), 3.141592653589793238);
  if (result.ec == std::errc{}) {
    std::println("Succ: {}", out);
  } else {
    std::println("Err: {}", result.ptr);
  }
  return 0;
}
```

## Converting from string
For the opposite conversion, that is, converting character sequence into numeric values, the following family of functions are available:

```cpp
std::from_chars_result from_chars(const char* first, const char* last, IntegerT& value, int base = 10);
std::from_chars_result from_chars(const char* first, const char* last, FloatT& value, std::chars_format format = chars_format::general);
```

Again, `std::chars_format` is defined as follows:

```cpp
struct from_chars_result {
	const char* ptr;
	errc ec;
};
```

The `ptr` member of the result is a pointer to the first character that wasn't converted, or equals `last` if all characters were successfully converted. If none of the character can be converted, `ptr` equals `first` and the value of error code would be `std::errc::invalid_argument`. If the converted value is too large to be represented by the given type, the value of the error code would be `std::errc::result_out_of_range`. Note that `std::from_chars` doesn't handle any leading white-space.

The following example demonstrate the perfect round-tripping feature of these low-level conversion functions:

```cpp
int main() {
  constexpr double origin{3.14159265};
  constexpr size_t BufferSize{128};
  std::string out(BufferSize, ' ');
  // from numeric to string
  auto [ptr1, err1]{std::to_chars(out.data(), out.data() + out.size(), origin)};
  if (err1 == std::errc{}) {
    std::println("Succ: {}", out);
  } else {
    std::println("Err: {}", ptr1);
  }
  // from string to numeric
  double val{};
  auto [ptr2, err2]{std::from_chars(out.data(), out.data() + out.size(), val)};
  if (err2 == std::errc{}) {
    std::println("Succ: {}", val);
  } else {
    std::println("Err: {}", ptr2);
  }
  if (val == origin) {
    std::println("Perfect round tripping");
  } else {
    std::println("No perfect round tripping");
  }
  return 0;
}
```

>[!NOTE] The above code can successfully executed in Visual Studio environment. When I executed it with Clang compiler, the compilation wouldn't passed. The reason for this is that libc++ doesn't provide the implementation for floating-point version of `std::from_chars`, whereas Visual Studio provides it. I don't test this code in GCC compiler, therefore I've no idea about whether GCC can pass compilation or not.
