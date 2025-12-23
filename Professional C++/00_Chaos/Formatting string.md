---
tags:
  - Professional-CPP
category:
  - Professional-CPP
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

