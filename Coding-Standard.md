Consistency is important, even more so when writing open-source code, since the code belongs to millions of eyeballs, and bug-fixing relies on these teeming millions to actually locate bugs and understand how to solve them.

This is why, when writing anything for OpenHub, be it a module or a core patch, you should strive to follow the following guidelines. They are the ones that OpenHub's developers adhere to, and following them is the surest way to have your code be elegantly integrated in OpenHub.

In short, code consistency helps keeping the code readable and maintainable.

## General conventions
All files containing code MUST:
  * Use only UTF-8 without BOM.
  * Use the Unix LF (linefeed) line ending.
  * End with a single blank line.

## PHP code conventions
PHP files MUST follow the PSR-2 standard. 

PHP CS Fixer has been configured for the OpenHub project to help developers to comply with these conventions.

You can run it using the following command:

## Javascript code conventions
Javascript files MUST follow the [Airbnb Javascript style guide](https://github.com/airbnb/javascript).