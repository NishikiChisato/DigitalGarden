---
tags:
  - book/Professional-CPP
  - CPP
  - status/seedlings
  - cpp/23
aliases:
  - format ranges
---
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
