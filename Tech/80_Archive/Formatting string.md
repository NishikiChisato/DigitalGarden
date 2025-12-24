---
tags:
  - Archived
Date Created: 2025-12-23 10:41:04
Date Modified: 2025-12-23 16:42:35
---
# Basic
The `std::format`, `std::print` and `std::println` use a *format string*, which specifies how the given argument must be formatted in the output string. The format string, containing a set of curly brackets `{}`, which represent a *replacement field*, is usually the first argument to these functions. We can have as many replacement fields as we need, and the subsequent argument to these function are values that are used to filled in these replacement fields. If we need `{` or `}`, we should escape them with `{{` or `}}`.

Inside these curly brackets(replacement field) can be a string in the format `[index][:specifier]`, as the follows shown:
- The optional `index` is argument index, discussed in [[#Argument Indices]].
- The optional `specifier` is a format specifier to control how a value must be formatted in the output string, discussed in [[#Format Specifiers]].

# Argument Indices
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

# Compile-Time Verification
## Format string
`std::format` and `std::vformat`

## Print to Console
`std::print/std::println` and `std::vprint_unicode/std::vprint_nonunicode`

# Format Specifiers
As mentioned earlier, a format string can contain replacement fields delimited by curly brackets, and the contain inside these curly brackets both have the same pattern: `[index][:specifier]`. The [[#Argument Indices]] section has described the `[index]` part of this pattern, this section is going to describe the `[:specifier]` part of this pattern.

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

## Formatting Escaped Characters and Strings

C++23 allows you to format escaped strings and characters by using the ? type specifier. This use case does not occur often, but it can be helpful for logging and debugging purposes. The output resembles how you write string and character literals in your code: they start and end with double or single quotes, and they use escaped character sequences. The following table shows what the output is of certain characters when using escaped formatting:

|    Character    | Escaped Output |
| :-------------: | :------------: |
| Horizontal tab  |       \t       |
|    New line     |       \n       |
| Carriage return |       \r       |
|    Backslash    |      \\\       |
|  Double quote   |      \\"       |
|  Single quote   |      \\'       |

The escaping of double quotes happens only when the output is a double-quoted string, while the escaping of single quotes happens only when the output is a single-quoted character. The escaped output of unprintable characters is `\u{hex-code-point}`.

Here are some examples:

```cpp
std::println("|{:?}|", "Hello\tWorld!\n"); // |Hello\tWorld!\n|
std::println("|{:?}|", "\""); // |"\""|
std::println("|{:?}|", '\''); // |'\''|
std::println("|{:?}|", '"');  // |'"'|
```

## Formatting Ranges
Starting with C++23, it’s possible to directly format such ranges of elements. For ranges such as `std::vector` and `std::array`, the output, by default, is surrounded by square brackets and individual elements are separated by commas. If the elements of the range are strings, their output is escaped by default.

The formatting of ranges can be controlled using nested format specifiers. The general form is as follows:

```
[[fill]align][width][n][range-type][:range-underlying-spec]
```

Everything between square brackets is optional. As with other format specifiers, fill specifies a fill character, align specifies the alignment of the output, and width specifies the width of the output field. If `n` is specified, the output will not contain the opening and closing brackets of the range. The range-type can be one of the following:

| Range-Type |                                                                                                                           Description                                                                                                                           |
| :--------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     m      | Available only for `std::pair` and `std::tuple` with two elements. By default, these are surrounded by parentheses and separated by commas. If `m` is specified, they are not surrounded by any type of brackets, and the two elements are separated by `": "`. |
|     s      |                                                                                    Formats the range as a string (cannot be combined with `n` or a *range-underlying-spec*).                                                                                    |
|     ?s     |                                                                               Formats the range as an escaped string (cannot be combined with `n` or a *range-underlying-spec*).                                                                                |

The *range-underlying-spec* is an optional format specifier for the individual elements of the range. Range specifiers can be nested multiple levels deep. If the elements are again ranges (e.g., a vector of vectors), then the *range-underlying-spec* is another range format specifier, and so on.

Let’s look at some examples. First, let’s format a vector of numbers:

```cpp
std::vector values { 11, 22, 33 };
std::println("{}", values);   // [11, 22, 33]
std::println("{:n}", values); // 11, 22, 33
```

If you want to replace the starting and ending square brackets, you can combine the n specifier with surrounding the format specifier with your own starting and ending characters. For example, the following surrounds the output with curly brackets instead. Curly brackets that you want to appear in the output need to be escaped as `{{` and `}}`.

```cpp
println("{{{:n}}}", values); // {11, 22, 33}
```

The following provides a format specifier for the entire range. For both, the range is output in the center of a field that is 16 characters wide with `*` as a fill character. For the second, the n specifies that the opening and closing brackets should be omitted:

```cpp
std::println("{:*^16}", values);  // **[11, 22, 33]**
std::println("{:*^16n}", values); // ***11, 22, 33***
```

The following does not provide an explicit specifier for the entire range, but it does specify how individual elements are to be formatted. In this case, the individual elements are output in the center of a field that’s six characters wide with `*` as a fill character:

```cpp
println("{::*^6}", values); // [**11**, **22**, **33**]
```

This can again be combined with the `n` specifier:

```cpp
std::println("{:n:*^6}", values); // **11**, **22**, **33**
```

Here are some examples formatting a vector of strings:

```cpp
using namespace std::string_literal;
std::vector strings { "Hello"s, "World!\t2023"s };
std::println("{}", strings);    // ["Hello", "World!\t2023"]
std::println("{:}", strings);   // ["Hello", "World!\t2023"]
std::println("{::}", strings);  // [Hello, World! 2023]
std::println("{:n:}", strings); // Hello, World! 2023
```

If you have a `vector` of characters, you can format them as individual characters, or you can consider the entire vector as a string using the `s` or `?s` range type:

```cpp
std::vector chars { 'W', 'o', 'r', 'l', 'd', '\t', '!' };
std::println("{}", chars);      // ['W', 'o', 'r', 'l', 'd', '\t', '!']
std::println("{::#x}", chars);  // [0x57, 0x6f, 0x72, 0x6c, 0x64, 0x9, 0x21]
std::println("{:s}", chars);    // World    !
std::println("{:?s}", chars);   // "World\t!"
```

Here are some examples of outputting a `std::pair`. By default, a `std::pair` is surrounded by parentheses instead of square brackets, and the two elements are separated by a comma. Using the `n` specifier removes the opening and closing parentheses. The `m` specifier also removes the parentheses and separates the elements with ": ".

```cpp
std::pair p { 11, 22 };
std::println("{}", p);   // (11, 22)
std::println("{:n}", p); // 11, 22
std::println("{:m}", p); // 11: 22
```

Finally, here are some examples of outputting a `vector` of `vector`s:

```cpp
std::vector<std::vector> vv { {11, 22}, {33, 44, 55} };
std::println("{}", vv);          // [[11, 22], [33, 44, 55]]
std::println("{:n}", vv);        // [11, 22], [33, 44, 55]
std::println("{:n:n}", vv);      // 11, 22, 33, 44, 55
std::println("{:n:n:*^4}", vv);  // *11*, *22*, *33*, *44*, *55*
```

