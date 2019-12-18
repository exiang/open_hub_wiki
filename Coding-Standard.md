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

PHP CS Fixer has been configured for the OpenHub project to help developers to comply with these conventions. As long as you are using visual studio code, it will auto run upon files save.

You may also manually run it using the following command:

<code></code>

## Javascript code conventions
Javascript files MUST follow the [Airbnb Javascript style guide](https://github.com/airbnb/javascript).

## HTML, CSS (Sass) code conventions
HTML, CSS (Sass) files MUST follow the [Mark Otto’s coding standards](http://codeguide.co/). Mark is the creator of the Bootstrap framework.

## License information
All OpenHub files MUST start with the OpenHub license block:

### Core Files
### Module files