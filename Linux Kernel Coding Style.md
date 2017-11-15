## Linux kernel coding style

Don't listen to the GNU coding standards.

1. Use tabs for indenting. Tabs are 8 characters wide. If you need to indent more than 3 levels, you've probably doing something wrong. Don't indent `case` labels beyond the level of it's `switch` statement. Don't put the body of a loop or conditional on the same line (BAD: `if (condition) do_this;`). Don't leave trailing whitespace.

2. Break longs lines so they fit on an 80 column screen, useless it's a user-visible `printk` message that someone might want to `grep`.

3. Opening braces don't get a separate line expect in function declarations. Closing braces do get a separate line unless followed by the continuation of the statement (e.g. `else`). If one branch of a conditional statement gets braces, then the other parts do too.

    Use spaces after keywords (for `if`, `switch`, `case`, `for`, `do`, `while`), but *not* after function names. Don't add spaces inside parentheses (BAD: `s = sizeof( struct file );`). Put `*` for pointers adjacent to the object name, not type name. Use spaces around operators.

4. Don't use camel-case, use hyphens and lowercase. Temporary variables should have short names, global variables should have descriptive names.

5. Don't use `typedef` for structs, *unless* 
(a) The object is totally opaque (e.g. objects that you can only access using the proper accessor functions). 
(b) The type depends on configuration (e.g. sometimes it's `unsigned int`, sometimes it's `unsigned long`). 
(c) If you're using `sparse` to create a new type for type checking.
(d) It's a type identical to standard C99 types (linux uses `u8`/`u16`/`u32`/`u64` instead of standard `uint32_t` types).
(e) Types that are safe for userspace.

6. Functions should be short and sweet, not more than 50-75 lines. Don't use more than 5-10 local variables. In function declarations, use the parameter name as well as the type name.

7. Use `goto` for centralizing the exiting of functions, but be careful that your exit code works on every path.

8. Remember to comment data. Comments on algorithms should explain *what* the code does, not *how* it does it. If you need a comment to explain how your code works, rewrite your code. Comments should go *outside* of functions. If you need to comment different parts of your functions separately, break your function into multiple functions. Consider also saying *why* your function does what it does.

Multi-line comments look like this:
```c
/*
 * This is the preferred style for multi-line
 * comments in the Linux kernel source code.
 * Please use it consistently.
 *
 * Description:  A column of asterisks on the left side,
 * with beginning and ending almost-blank lines.
 */
```

For files in `net/` and `drivers/net/`, comments look like this:
```c
/* The preferred comment style for files in net/ and drivers/net
* looks like this.
*
* It is nearly the same as the generally preferred comment style,
* but there is no initial almost-blank line.
*/
```

9. Don't use emacs / indent defaults.

10. Kconfig files are indented differently.

11. There's no such thing as garbage collection in the kernel, so you have to do your own garbage collection. Count references to objects.

12. Macro constants and enum labels should be CAPITALIZED. Macros resembling functions can be lower-case, but consider using an inline function instead. Enclose macros with multiple statements in a do - while block. Never use macros as l-values. Remember your parentheses in macros. Use double-underscores and prefixes for macros that declare variables to avoid namespace collision (a macro called `FOO` should define a variable as `__foo_ret` instead of `ret`).

13. Spell check your `printk` messages. Don't bother terminating messages with a period. Don't print numbers in parentheses. Some messages should only be printed when `DEBUG` is defined.

14. When using `kmalloc` to allocate memory for an object pointed to by `p`, use `*p` as your argument to `sizeof`, like this: `p = kmalloc(sizeof(*p), ...);`. If you use the type of `*p` as the argument (`p = kmalloc(sizeof(struct p_struct), ...);`), then this needs to be changed if the type of `*p` changes. Don't bother casting the `void *` pointer that `kmalloc` returns, this is done automatically. Use `kmalloc_array` and `kcalloc` for non-zeroed and zeroed arrays, respectively.

15. Inline functions can be useful replacements for macros, but don't go overboard. Too much inlining increases the size of the kernel which slows down the system because it increases the icache footprint and decreases the memory available for the pagecache. If a function is used only once, you might think that inlining is a no-brainer because there's no space trade off. *Don't* do this, gcc knows how to do this automatically already, and if the function gets used more than once later, someone has to remove the inline.

16. Procedures, whose names are commands (like `add_work()`) should return `0` for success and an error code for failure. Functions, whose names are predicates (like `pci_dev_present()`) should return `true` for success and `false` for failure.

17. Read the `include/linux/kernel.h` header file, it contains lots of useful macros. Don't reinvent these.

18. Some editors like emacs and vim allow you to embed configuration information for a source file in the file itself (like `/* vim:set sw=8 noet */`). Don't include these in source files.

19. Don't use inline assembly if C will do, you can poke hardware with C. Consider wrapping assembly in helper functions and making use of C parameters. Large assembly functions should go in .S files, and C prototypes for these functions should be declared in C header files with the `asmlinkage` tag. You might need mark `asm` statements as `volatile` to prevent the complier from optimizing them away, but only do this if you know you need to otherwise it messes with optimization. Make use of the compiler's automatic concatenation of adjacent strings for multiple assembly instructions in a single `asm` statement. Separate them with a newline like this:
```c
asm ("magic %reg1, #42\n\t"
     "more_magic %reg2, %reg3"
     : /* outputs */ : /* inputs */ : /* clobbers */);
```

20. Don't use preprocessor conditionals in .c files if you can avoid it. Instead, conditionally define no-op versions of functions in header files and call these functions unconditionally in .c files. The compilers will optimize away these no-op functions, so the generated code is identical. Put code that gets conditionally compiled into it's own function for this purpose. If a compiler is warning you about a function that's unused in a certain configuration, mark it as `__maybe_unused` instead of using preprocessor conditionals.
