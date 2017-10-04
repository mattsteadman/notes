### [Notes on Programming in C](http://doc.cat-v.org/bell_labs/pikestyle) by Rob Pike (1989)

- Consider the relationship between variable name length and the frequency that it is used. It's acceptable for a rarely used global variable to have a long name (say `maxphysaddr`), but something like a dummy variable for looping should be something simple (like `i`).

- Symmetry is a virture in variable names. If if you have a variable called `maxphysaddr`, you should name it's counterpart `minphysaddr`, *not* `lowestphysaddr`.

- Rob Pike doesn't like camel case.

- Chose pointers that point to something as close to the object you want to access as possible. It's better to write `lp->type` than `parent->link[i].type`. If you need to access the next element in the link array, you can always do `(++lp)->type` instead of `parent->link[++i].type`.

- Procedure names should reflect what they *do*; function names should reflect what they *return*. Consider `if(checksize(x))` vs `if(validsize(x))`. The latter is preferred because we are naming a function, and it returns whether `x` is a valid size.

- Comment that explain how data structures are used are generally more useful than comments that explain algorithms.

- Reducing microscopic complexity of a program is more straightforward than reducing macrosope complexity (i.e. complexity that is a result of overall program design). Don't try to do fancy things (like use splay trees) when something simple (like binary trees) is available. Most of the time, fancy tricks don't improve anything because `n` is small. Fancy algorithms are buggier than simple ones and take more time to implement. Almost all practical problems can be solved with arrays, linked lists, hash tables, and binary trees.

- Data structures, not algorithms, are central to programming. If you chose the right data structures, the algorithms will fall into place.

- Often, the details of an algorithm can be encoded in data rather than code. He gives the example of generating parsing tables, which encodes the grammar of a language in a form that is easily interpreted by a simple program. He calls these kind of algorithms *data-drive*. Languages that try to firmly separate code and data (like Pascal) can make these kind of approach more difficult. Turing and Von Neumann knew that code and data are *the same thing* (or can be).

- Function pointers is a great way is distributing complexity to the different functions that can be pointed to for a particular function call. To make this work, functions that are used for some particular purpose need to obey some standard protocol. ("All functions used similarly must behave similarly.") The clear use of function pointers is the heart of object-oriented programming. Grouping data together with functions that operate on it is good design. Object-oriented languages give you nicer syntax for doing this, but you can still make use of this design pattern just as well in C.

- Combining data-driven programming with effective use of function pointers can lead to very expressive code. In C, you can get most of the benefit of object-oriented programming by using this approach without sacrificing the extra control that using C provides.

- Include files should never include other include files. This is how to avoid the problem of multiple inclusions. Instead, tell the user which other include files need to be included before including some particular include file.
