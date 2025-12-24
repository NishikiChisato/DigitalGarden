---
tags:
  - book/Professional-CPP
  - CPP
  - cpp/23
  - status/evergreen
aliases:
  - format escaped characters and strings
---
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
