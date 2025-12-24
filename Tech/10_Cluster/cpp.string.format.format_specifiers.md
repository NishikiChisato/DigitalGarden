---
tags:
  - book/Professional-CPP
  - CPP
  - status/seedlings
aliases:
  - format specifier
  - format specifiers
---
# Format Specifiers
As mentioned earlier, a format string can contain replacement fields delimited by curly brackets, and the contain inside these curly brackets both have the same pattern: `[index][:specifier]`. The [[cpp.string.format.argument_index|argument indices]] section has described the `[index]` part of this pattern, this section is going to describe the `[:specifier]` part of this pattern.

The general form of specifier is as follows:

```
[[fill]align][sign][#][0][width][.precision][L][type]
```

Note that all parts between square brackets are optional.

>[!NOTE] The following content both are copied from the origin book, since I think we're unnecessary to interpret it with my own words.
## width

The width specifies the minimum width of the field into which the given value should be formatted. This can also be another set of curly brackets, in which case it’s called a *dynamic width*. If an index is specified in the curly brackets, for example `{3}`, the value for the dynamic width is taken from the argument with the given index. Otherwise, if no index is specified, for example `{}`, the width is taken from the next argument in the list of arguments.

Here are some examples:

```cpp
int i { 42 };
std::println("|{:5}|", i);       // |   42|
std::println("|{:{}}|", i, 7);   // |     42|
std::println("|{1:{0}}|", 7, i); // |     42|
```

## \[fill\]align

The \[fill\] align part optionally says what character to use as a fill character, followed by how a value should be aligned in its field:
- `<` means left alignment (default for non-integers and non-floating-point numbers).
- `>` means right alignment (default for integers and floating-point numbers).
- `^` means center alignment.

The fill character is inserted into the output to make sure the field in the output reaches the desired minimum width specified by the \[width\] part of the specifier. If no \[width\] is specified, then \[fill\] align has no effect. When using center alignment, the same number of fill characters is on the left and on the right of the formatted value. If the total number of fill characters is odd, then the extra fill character is added on the right. Here are some examples:

```cpp
int i { 42 };
std::println("|{:7}|", i);   // |     42|
std::println("|{:<7}|", i);  // |42     |
std::println("|{:_>7}|", i); // |_____42|
std::println("|{:_^7}|", i); // |__42___|
```

The following is an interesting trick to output a character a specific number of times. Instead of typing a string literal ourself containing the correct number of characters, we specify the number of characters we need explicitly in the format specifier:

```cpp
println("|{:=>16}|", ""); // |================|
```

## sign

The sign part can be one of the following:
- `-` means to only display the sign for negative numbers (default).
- `+` means to display the sign for negative and positive numbers.
- `space` means that a minus sign should be used for negative numbers, and a space for positive numbers.

Here are some examples:

```cpp
int i { 42 };
std::println("|{:<5}|", i);   // |42   |
std::println("|{:<+5}|", i);  // |+42  |
std::println("|{:< 5}|", i);  // | 42  |
std::println("|{:< 5}|", -i); // |-42  |
```

## \#

The # part enables the alternate formatting rules. If enabled for integral types, and hexadecimal, binary, or octal number formatting is specified as well, then the alternate format inserts a `0x`, `0X`, `0b`, `0B`, or `0` in front of the formatted number. If enabled for floating-point types, the alternate format will always output a decimal separator, even if no digits follow it. The following two sections([[#type]] and [[#precision]]) give examples with alternate formatting.

## type

The type specifies the type a given value must be formatted in. There are several options:
- Integer types: `b` (binary), `B` (binary, but with `0B` instead of `0b` if `#` is specified), `d` (decimal), `o` (octal), `x` (hexadecimal with lowercase `a, b, c, d, e, f`), `X` (hexadecimal with uppercase `A, B, C, D, E, F`, and if `#` is specified, with `0X` instead of `0x`). If type is unspecified, `d` is used for integer types.
- Floating-point types: The following floating-point formats are supported. The result of scientific, fixed, general, and hexadecimal formatting is the same as discussed earlier in this chapter for std::chars_format::scientific, fixed, general, and hex.
	- `e, E`: Scientific notation with either small `e` or capital `E` as the representation of the exponent, formatted with either a given precision or 6 if no precision is specified.
	- `f, F`: Fixed notation formatted with either a given precision or 6 if no precision is specified.
	- `g, G`: General notation automatically chooses a representation without an exponent (fixed format) or with an exponent (small e or capital E), formatted with either a given precision or 6 if no precision is specified.
	- `a, A`: Hexadecimal notation with either lowercase letters (a) or uppercase letters (A)
	- If `type` is unspecified, `g` is used for floating-point types.
- Booleans: `s` (outputs true or false in textual form), `b`, `B`, `c`, `d`, `o`, `x`, `X` (outputs 1 or 0 in integer form). If type is unspecified, `s` is used for Boolean types.
- Characters: `c` (character is copied to output), `?` (escaped character is copied to output), `b`, `B`, `d`, `o`, `x`, `X` (integer representation). If type is unspecified, `c` is used for character types.
- String: `s` (string is copied to output), `?` (escaped string is copied to output). If type is unspecified, `s` is used for string types.
- Pointers: `p` (hexadecimal notation of the pointer prefixed with `0x`). If type is unspecified, `p` is used for pointer types. Only pointers of type `void*` can be formatted. Other pointer types must first be converted to type `void*`, for example using `static_cast<void*>(myPointer)`.

Here are some examples with an integral type:

```cpp
int i { 42 };
std::println("|{:10d}|", i);  // |        42|
std::println("|{:10b}|", i);  // |    101010|
std::println("|{:#10b}|", i); // |  0b101010|
std::println("|{:10X}|", i);  // |        2A|
std::println("|{:#10X}|", i); // |      0X2A|
```

Here is an example with a string type:

```cpp
std::string s { "ProCpp" };
std::println("|{:_^10}|", s); // |__ProCpp__|
```

## precision

The `precision` can be used only for floating-point and string types. It is specified as a dot followed by the number of decimal digits to output for floating-point types, or the number of characters to output for strings. The number of digits for floating-point types includes all digits, including the ones before the decimal separator, unless fixed floating-point notation (`f` or `F`) is used, in which case `precision` is the number of digits after the decimal point.

Just as with `width`, `precision` can also be another set of curly brackets, in which case it’s called a *dynamic precision*. The precision is then taken either from the next argument in the list of arguments or from the argument with given index.

Here are some examples using a floating-point type:
```cpp
double d { 3.1415 / 2.3 };
std::println("|{:12g}|", d);                          // |     1.36587|
std::println("|{:12.2}|", d);                         // |         1.4|
std::println("|{:12e}|", d);                          // |1.365870e+00|
int width { 12 };
int precision { 3 };
std::println("|{2:{0}.{1}f}|", width, precision, d);  // |       1.366|
std::println("|{2:{0}.{1}}|", width, precision, d);   // |        1.37|
```

## 0

The `0` part of the specifier means that, for numeric values, zeros are inserted into the formatted value to reach the desired minimum width specified by the \[width\] part of the specifier (see earlier). These zeros are inserted at the front of the numeric value, but after any sign, and after any `0x`, `0X`, `0b`, or `0B` prefix. The `0` specifier is ignored if an alignment is specified.

Here are some examples:

```cpp
int i { 42 };
std::println("|{:06d}|", i);  // |000042|
std::println("|{:+06d}|", i); // |+00042|
std::println("|{:06X}|", i);  // |00002A|
std::println("|{:#06X}|", i); // |0X002A|
```

## L

The optional `L` specifier enables locale-specific formatting. This option is valid only for arithmetic types, such as integers, floating-point types, and Booleans. When used with integers, the `L` option specifies that the locale-specific digit group separator character must be used. For floating-point types, it means to use the locale-specific digit group and decimal separator characters. For Boolean types output in textual form, it means to use the locale-specific representation of true and false. When using the `L` specifier, you have to pass an `std::locale` instance as the first parameter to `std::format()`. This works only with `std::format()`, not with `std::print()` and `std::println()`.

Here is an example that formats a floating-point number using the `nl` locale:

```cpp
float f { 1.2f };
cout << format(std::locale{ "nl" }, "|{:Lg}|\n", f); // |1,2|
```
