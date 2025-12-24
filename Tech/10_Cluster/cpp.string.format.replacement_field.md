---
tags:
  - book/Professional-CPP
  - CPP
  - status/seedlings
aliases:
  - replacement field
  - replacement fields
---
# Basic
The `std::format`, `std::print` and `std::println` use a *format string*, which specifies how the given argument must be formatted in the output string. The format string, containing a set of curly brackets `{}`, which represent a *replacement field*, is usually the first argument to these functions. We can have as many replacement fields as we need, and the subsequent argument to these function are values that are used to filled in these replacement fields. If we need `{` or `}`, we should escape them with `{{` or `}}`.

Inside these curly brackets(replacement field) can be a string in the format `[index][:specifier]`, as the follows shown:
- The optional `index` is argument index, discussed in [[cpp.string.format.argument_index|argument indices]].
- The optional `specifier` is a format specifier to control how a value must be formatted in the output string, discussed in [[cpp.string.format.format_specifiers|format specifiers]].