---
tags:
  - book/Professional-CPP
  - CPP
Date Created: 2025-12-23 15:31:11
Date Modified: 2025-12-23 15:32:40
---
>[!NOTE] This section is copied from the original book, since I think interpret it with my own words is unnecessary.

Raw string literals are string literals that can span multiple lines of code, don’t require escaping of embedded double quotes, and process escape sequences like \t and \n as normal text and not as escape sequences. With a raw string literal, we can avoid the need to escape the quotes. A raw string literal starts with R"( and ends with )":

`std::println(R"(Hello "World"!)");`

If we need a string consisting of multiple lines, without raw string literals, we need to embed `\n` escape sequences in our string where we want to start a new line. Here’s an example:

`println("Line 1\nLine 2");`

The output is as follows:
```
Line 1
Line 2
```

With a raw string literal, instead of using `\n` escape sequences to start new lines, we can simply press Enter to start real physical new lines in our source code as follows. The output is the same as the previous code snippet using the embedded `\n`.

```cpp
println(R"(Line 1
Line 2)");
```

Escape sequences are ignored in raw string literals. For example, in the following raw string literal, the `\t` escape sequence is not replaced with a tab character but is kept as the sequence of a backslash followed by the letter t:

```cpp
println(R"(Is the following a tab character? \t)"); 
```

This outputs the following: `Is the following a tab character? \t`

Because a raw string literal ends with `)"`, we cannot embed a `)"` in our string using this syntax. For example, the following string is not valid because it contains the `)"` sequence in the middle of the string:

```cpp
println(R"(Embedded )" characters)"); // Error!
```

If we need embedded `)"` characters, we need to use the extended raw string literal syntax, which is as follows:

```
R"d-char-sequence(r-char-sequence)d-char-sequence"
```

The `r-char-sequence` is the actual raw string. The `d-char-sequence` is an optional delimiter sequence, which should be the same at the beginning and at the end of the raw string literal. This delimiter sequence can have at most 16 characters. we should choose this delimiter sequence as a sequence that will not appear in the middle of our raw string literal.

The previous example can be rewritten using a unique delimiter sequence as follows: `std::println(R"-(Embedded )" characters)-");`
