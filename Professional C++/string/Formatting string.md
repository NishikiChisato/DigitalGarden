---
tags:
  - CPP
  - Professional-CPP
category:
  - Professional-CPP
Date Created: 2025-12-23 10:41:04
Date Modified: 2025-12-23 14:23:52
---
# Basic
The `std::format`, `std::print` and `std::println` use a *format string*, which specifies how the given argument must be formatted in the output string. The format string, containing a set of curly brackets `{}`, which represent a *replacement field*, is usually the first argument to these functions. We can have as many replacement fields as we need, and the subsequent argument to these function are values that are used to filled in these replacement fields. If we need `{` or `}`, we should escape them with `{{` or `}}`.

Inside these curly brackets(replacement field) can be a string in the format `[index][:specifier]`, as the follows shown:
- The optional `index` is argument index, discussed in [[#Argument Indices]].
- The optional `specifier` is a format specifier to control how a value must be formatted in the output string, discussed in [[#Format Specifiers]].

# Argument Indices



# Compile-Time Verification



# Format Specifiers


