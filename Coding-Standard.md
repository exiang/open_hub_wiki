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

PHP CS Fixer has been configured for the OpenHub project to help developers to comply with these conventions. As long as you are using visual studio code, it will auto run upon file save. Please make sure you installed the right extension: php cs fixer from junstyle.

![](https://user-images.githubusercontent.com/5336690/81776895-49ee4480-9522-11ea-8e54-2dad561ded4f.png)

Also make sure this extension is set as the defaultFormatter in Visual Studio Code user's setting.

```
// Place your settings in this file to overwrite the default settings
{
    "files.autoSave": "off",
    "workbench.editor.enablePreview": false,
    "editor.mouseWheelZoom": true,
    "editor.wordWrap": "on",
    "window.zoomLevel": 0,
    "[php]": {
        "editor.defaultFormatter": "junstyle.php-cs-fixer"
    },
    "git.ignoreMissingGitWarning": true,
    "php-cs-fixer.exclude": [
    
    ]
}
```

The location of this file is at `/.php_cs`

Content of the PHP cs fixer file for your reference:
``` php
<?php
$finder = PhpCsFixer\Finder::create()
	->exclude(array('framework', 'public_html/requirements','public_html/themes','protected/messages'))
	->in(__DIR__);

return PhpCsFixer\Config::create()
    ->setFinder($finder)
    ->setRules(array(
        '@PSR2' => true,
        //'array_indentation' => true,
        //'array_syntax' => array('syntax' => 'short'),
        'combine_consecutive_unsets' => true,
        'method_separation' => true,
        'no_multiline_whitespace_before_semicolons' => true,
        'single_quote' => true,

        'binary_operator_spaces' => array(
            'align_double_arrow' => false,
            'align_equals' => false,
        ),
        'blank_line_after_opening_tag' => true,
        'blank_line_before_return' => true,
        'braces' => array(
            'allow_single_line_closure' => true,
            'position_after_functions_and_oop_constructs' => 'next',
        ),
        // 'cast_spaces' => true,
        // 'class_definition' => array('singleLine' => true),
        'concat_space' => array('spacing' => 'one'),
        'declare_equal_normalize' => true,
        'function_typehint_space' => true,
        'hash_to_slash_comment' => true,
        'include' => true,
        'lowercase_cast' => true,
        // 'native_function_casing' => true,
        // 'new_with_braces' => true,
        // 'no_blank_lines_after_class_opening' => true,
        // 'no_blank_lines_after_phpdoc' => true,
        // 'no_empty_comment' => true,
        // 'no_empty_phpdoc' => true,
        // 'no_empty_statement' => true,
        'no_extra_consecutive_blank_lines' => array(
            'curly_brace_block',
            'extra',
            'parenthesis_brace_block',
            'square_brace_block',
            'throw',
            'use',
        ),
        // 'no_leading_import_slash' => true,
        // 'no_leading_namespace_whitespace' => true,
        // 'no_mixed_echo_print' => array('use' => 'echo'),
        'no_multiline_whitespace_around_double_arrow' => true,
        // 'no_short_bool_cast' => true,
        // 'no_singleline_whitespace_before_semicolons' => true,
        'no_spaces_around_offset' => true,
        // 'no_trailing_comma_in_list_call' => true,
        // 'no_trailing_comma_in_singleline_array' => true,
        // 'no_unneeded_control_parentheses' => true,
        // 'no_unused_imports' => true,
        'no_whitespace_before_comma_in_array' => true,
        'no_whitespace_in_blank_line' => true,
        // 'normalize_index_brace' => true,
        'object_operator_without_whitespace' => true,
        // 'php_unit_fqcn_annotation' => true,
        // 'phpdoc_align' => true,
        // 'phpdoc_annotation_without_dot' => true,
        // 'phpdoc_indent' => true,
        // 'phpdoc_inline_tag' => true,
        // 'phpdoc_no_access' => true,
        // 'phpdoc_no_alias_tag' => true,
        // 'phpdoc_no_empty_return' => true,
        // 'phpdoc_no_package' => true,
        // 'phpdoc_no_useless_inheritdoc' => true,
        // 'phpdoc_return_self_reference' => true,
        // 'phpdoc_scalar' => true,
        // 'phpdoc_separation' => true,
        // 'phpdoc_single_line_var_spacing' => true,
        // 'phpdoc_summary' => true,
        // 'phpdoc_to_comment' => true,
        // 'phpdoc_trim' => true,
        // 'phpdoc_types' => true,
        // 'phpdoc_var_without_name' => true,
        // 'pre_increment' => true,
        // 'return_type_declaration' => true,
        // 'self_accessor' => true,
        // 'short_scalar_cast' => true,
        'single_blank_line_before_namespace' => true,
        // 'single_class_element_per_statement' => true,
        // 'space_after_semicolon' => true,
        // 'standardize_not_equals' => true,
        'ternary_operator_spaces' => true,
        // 'trailing_comma_in_multiline_array' => true,
        'trim_array_spaces' => true,
        'unary_operator_spaces' => true,
        'whitespace_after_comma_in_array' => true,
    ))
    ->setIndent("\t")
    ->setLineEnding("\n")
;
```
### Bulk fix
In command line at the specific directory to fix all PHP files in it, type command `php-cs-fixer fix`.

![](https://user-images.githubusercontent.com/5336690/81777341-3394b880-9523-11ea-90af-4130cd36726d.png)


<code></code>

## Javascript code conventions
Javascript files MUST follow the [Airbnb Javascript style guide](https://github.com/airbnb/javascript).

## HTML, CSS (Sass) code conventions
HTML, CSS (Sass) files MUST follow the [Mark Otto’s coding standards](http://codeguide.co/). Mark is the creator of the Bootstrap framework.

## License information
All OpenHub files MUST start with the OpenHub license block:

### Core Files
```
/**
*
* NOTICE OF LICENSE
*
* This source file is subject to the BSD 3-Clause License
* that is bundled with this package in the file LICENSE.
* It is also available through the world-wide-web at this URL:
* https://opensource.org/licenses/BSD-3-Clause
*
*
* @author Malaysian Global Innovation & Creativity Centre Bhd <tech@mymagic.my>
* @link https://github.com/mymagic/open_hub
* @copyright 2017-2020 Malaysian Global Innovation & Creativity Centre Bhd and Contributors
* @license https://opensource.org/licenses/BSD-3-Clause
*/
```
### Module files