[![Build Status](https://travis-ci.org/MarcusSchwarz/lesserphp.svg?branch=master)](https://travis-ci.org/MarcusSchwarz/lesserphp) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/MarcusSchwarz/lesserphp/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/MarcusSchwarz/lesserphp/?branch=master) [![Coverage Status](https://coveralls.io/repos/github/MarcusSchwarz/lesserphp/badge.svg?branch=master)](https://coveralls.io/github/MarcusSchwarz/lesserphp?branch=master)

# lesserphp dev-master
### <http://github.com/MarcusSchwarz/lesserphp>
**Please note:** Please bear in mind, the master branch is *not* up-to-date with the release branch, which is 0.6-dev. Bug fixes should go to branch 0.6-dev (php7.2 and greater) or 0.5-dev (php5.6 to php7.4). Thank you!

`lesserphp` is a compiler for LESS written in PHP. It is based on lessphp bei leafo.
The documentation is great,
so check it out: <https://www.maswaba.de/lesserphpdocs/>.

Here's a quick tutorial:

### How to use in your PHP project

The only file required is `lessc.inc.php`, so copy that to your include directory.

The typical flow of **lesserphp** is to create a new instance of `lessc`,
configure it how you like, then tell it to compile something using one built in
compile methods.

The `compile` method compiles a string of LESS code to CSS.

```php
<?php
require "lessc.inc.php";

$less = new lessc;
echo $less->compile(".block { padding: 3 + 4px }");
```

The `compileFile` method reads and compiles a file. It will either return the
result or write it to the path specified by an optional second argument.

```php
<?php
echo $less->compileFile("input.less");
```

The `checkedCompile` method is like `compileFile`, but it only compiles if the output
file doesn't exist or it's older than the input file:

```php
<?php
$less->checkedCompile("input.less", "output.css");
```

If there any problem compiling your code, an exception is thrown with a helpful message:

```php
<?php
try {
  $less->compile("invalid LESS } {");
} catch (\Exception $e) {
  echo "fatal error: " . $e->getMessage();
}
```

The `lessc` object can be configured through an assortment of instance methods.
Some possible configuration options include [changing the output format][1],
[setting variables from PHP][2], and [controlling the preservation of
comments][3], writing [custom functions][4] and much more. It's all described
in [the documentation][0].


 [0]: https://www.maswaba.de/lesserphpdocs/
 [1]: https://www.maswaba.de/lesserphpdocs/#output_formatting
 [2]: https://www.maswaba.de/lesserphpdocs/#setting_variables_from_php
 [3]: https://www.maswaba.de/lesserphpdocs/#preserving_comments
 [4]: https://www.maswaba.de/lesserphpdocs/#custom_functions


### How to use from the command line

An additional script has been included to use the compiler from the command
line. In the simplest invocation, you specify an input file and the compiled
css is written to standard out:

    $ plessc input.less > output.css

Using the -r flag, you can specify LESS code directly as an argument or, if
the argument is left off, from standard in:

    $ plessc -r "my less code here"

Finally, by using the -w flag you can watch a specified input file and have it
compile as needed to the output file:

    $ plessc -w input-file output-file

Errors from watch mode are written to standard out.

The -f flag sets the [output formatter][1]. For example, to compress the
output run this:

    $ plessc -f=compressed myfile.less

For more help, run `plessc --help`

