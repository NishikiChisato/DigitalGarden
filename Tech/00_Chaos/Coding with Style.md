---
tags:
  - CPP
  - book/Professional-CPP
---
# Reason to write comments

## Commenting to Explain Usage
```cpp
// Saves the given record to the database.
// Parameters:
// record:If the given record doesn't yet have adatabase ID, then saveRecord()
// modifies the record object to store the ID assiigned by the database.
// Returns: int
// An integer representing the ID of the savedrecord.
// Throws:
// DatabaseNotOpenedException if openDatabase ()has not been called yet.
int saveRecord(Record& record);
```

The previous comment documents everything about saveRecord() in a formal way, including a sentence that describes what the member function does. Some companies require such formal and thorough documentation; however, I don’t recommend this style of commenting all the time. The first line, for example, is rather useless since the name of the function is self-explanatory. The description of the parameter is important as is the comment about the exception, so these definitely should stay.

Documenting what exactly the return type represents for this version of saveRecord() is required since it returns a generic int. However, a much better design would be to return a RecordID instead of a plain int, which removes the need to add any comments for the return type. RecordID could be a simple class with a single int data member, but it conveys more information, and it allows you to add more data members in the future if need be. So, the following is a much better saveRecord():

```cpp
// Parameters:
// record: If the given record doesn't yet have a database ID, then saveRecord()
// modifies the record object to store the ID asssigned by the database.
// DatabaseNotOpenedException if openDatabase()has not been called yet.
// Throws:
// DatabaseNotOpenedException if openDatabase() has not been called yet.
RecordID saveRecord(Record& record);
```

Your public documentation should describe the behavior of your code, not the implementation. The behavior includes the inputs, outputs, error conditions and handling, intended uses, and performance guarantees. For example, public documentation describing a call to generate a single random number should specify that it takes no parameters, returns an integer in a previously specified range, and should list all the exceptions that might be thrown when something goes wrong. This public documentation should not explain the details of the linear congruence algorithm for actually generating the number. Providing too much implementation detail in comments targeted for users of your code is probably the single most common mistake in writing public comments.

## Commenting to Explain Complicated Code

## Commenting to Convey Meta-information

# Commenting Styles

## Commenting Every Line

## Prefix Comments

## Ad Hoc Comments

## Self-Documenting Code

# DECOMPOSITION

Decomposition is the practice of breaking up code into smaller pieces. There is nothing more daunting in the world of coding than opening up a file of source code to find 300-line functions and massive, nested blocks of code. Ideally, each function should accomplish a single task. Any subtasks of significant complexity should be decomposed into separate functions. For example, if somebody asks you what a function does and you answer, “First it does A, then it does B; then, if C, it does D; otherwise, it does E,” you should probably have separate helper functions for A, B, C, D, and E.

Decomposition is not an exact science. Some programmers will say that no function should be longer than a page of printed code. That may be a good rule of thumb, but you could certainly find a quarter-page of code that is desperately in need of decomposition. Another rule of thumb is that if you squint your eyes and look at the format of the code without reading the actual content, it shouldn’t appear too dense in any one area.


Whether your code starts its life as a dense block of unreadable cruft or it just evolves that way, refactoring is necessary to periodically purge the code of accumulated hacks. Through refactoring, you revisit existing code and rewrite it to make it more readable and maintainable. Refactoring is an opportunity to revisit the decomposition of code. If the purpose of the code has changed or if it was never decomposed in the first place, when you refactor the code, squint at it and determine whether it needs to be broken down into smaller parts.

When refactoring code, it is important to be able to rely on a testing framework that catches any defects that you might introduce. Unit tests, discussed in Chapter 30, are particularly well suited for helping you catch mistakes during refactoring.

# Naming
The best name for a variable, member function, function, parameter, class, namespace, and so on, accurately describes the purpose of the item. Names can also imply additional information, such as the type or specific usage. Of course, the real test is whether other programmers understand what you are trying to convey with a particular name.

There are no set-in-stone rules for naming other than the rules that work for your organization. However, there are some names that are rarely appropriate.

## Name Conventions
### Prefix
Many programmers begin their variable names with a letter that provides some information about the variable’s type or usage. On the other hand, there are as many programmers, or even more, who disapprove of using any kind of prefix because this could make evolving code less maintainable in the future. For example, if a member variable is changed from static to non-static, you have to rename all the uses of that name. If you don’t rename them, your names continue to convey semantics, but now they are the wrong semantics.

### Getters and Setters
If your class contains a data member, such as m_status, it is customary to provide access to the member via a getter called getStatus() and, optionally, a setter called setStatus(). To give access to a Boolean data member, you typically use is as a prefix instead of get, for example isRunning(). The C++ language has no prescribed naming for these functions, but your organization will probably want to adopt this or a similar naming scheme.

### Capitalization
There are many different ways of capitalizing names in your code. As with most elements of coding style, it is important that your group adopts a standardized approach and that all members adopt that approach. One way to get messy code is to have some programmers naming classes in all lowercase with underscores representing spaces (priority_queue) and others using capitals with each subsequent word capitalized (PriorityQueue). Variables and data members almost always start with a lowercase letter and use either underscores (my_queue) or capitals (myQueue) to indicate word breaks. Functions traditionally start with a capital letter in C++, but, as you’ve seen, in this book I have adopted the style of using a lowercase first letter for functions to distinguish them from class names.



# Formatting
If no standards are in place for code formatting, I recommend introducing them in your organization. Standardized coding guidelines make sure that all programmers on your team follow the same naming conventions, formatting rules, and so on, which makes the code more uniform and easier to understand.

If everybody on your team is just writing code their own way, try to be as tolerant as you can. As you’ll see, some practices are just a matter of taste, while others actually make it difficult to work in teams.

## Space, Tabs, and Line Breaks
The use of spaces and tabs is not merely a stylistic preference. If your group does not agree on a convention for spaces and tabs, there are going to be major problems when programmers work jointly. The most obvious problem occurs when Alice uses four-space tabs to indent code and Bob uses five-space tabs; neither will be able to display code properly when working on the same file. An even worse problem arises when Bob reformats the code to use tabs at the same time that Alice edits the same code; many version control systems won’t be able to merge in Alice’s changes.

Most, but not all, editors have configurable settings for spaces and tabs. Some environments even adapt to the formatting of the code as it is read in or always save using spaces even if the Tab key is used for authoring. If you have a flexible environment, you have a better chance of being able to work with other people’s code. **Just remember that tabs and spaces are different because a tab can be any length and a space is always a space.**


# Conclusion

>[!QUOTE]
> Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live. Code for readability.
> 
> John F. Woods, Sep 24, 1991, COMP.LANG.C++

