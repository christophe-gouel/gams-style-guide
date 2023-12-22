# GAMS Style Guide #

This file describes a style guide for the GAMS programming language. It takes inspiration from the [tidyverse style guide](https://style.tidyverse.org/) for the R programming language (with copied elements authorized by the associated [license](https://github.com/tidyverse/style/blob/main/LICENSE.md)).

This guide is necessarily opinionated in order to provide consistency. Feel free to adjust it to the style you want to adopt in your research group.

Other available style guides for GAMS:

- [MIT JPSPGC style guide](https://github.com/mit-jp/style-guides)
- [Good coding practices from GAMS documentation](https://www.gams.com/latest/docs/UG_GoodPractices.html), based on [Bruce McCarl's recommendations](https://www.gams.com/mccarlGuide/formatting_models_-_my_recommendations.htm)

**Author**: [Christophe Gouel](https://github.com/christophe-gouel)

## Spacing ##

Do not place spaces before comma and semicolon.

### Commas ###

Always put a space after a comma, except for the commas separating sets in a GAMS object and indexed functions such as `sum` but separate sets by a space in functions such as `sameAs` or declarations such as `Alias`.

``` gams
* Good
Alias (i, j);
eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst));
z(i,j) = yes $ (not sameAs(i, j));

* Bad
Alias (i,j);
eq_p .. p =e= sum((i, j),sh_g(i, j) * p_g(i, j)**(1 - subst))**(1 / (1 - subst));
z(i,j) = yes $ (not sameAs(i,j));
```

### Infix operators ###

Most infix operators (`==`, `+`, `-`, `/`, `=e=`, etc.) should always be surrounded by spaces. This rule also applies to `$` used for conditional assignments and to the two dots `..` used for defining equations.

``` gams
# Good
eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst));
z(i,j) = yes $ (not sameAs(i, j));

# Bad
eq_p.. p=e=sum((i,j), sh_g(i,j)*p_g(i,j)**(1-subst))**(1/(1-subst));
z(i,j) = yes$(not sameAs(i, j));
```

There are a few exceptions, which should never be surrounded by spaces:

- The operators with high precedence: exponentiation `**`, unary `-`, unary `+`, and `*` used as a sequence operator.

``` gams
* Good
Set t / 1*100 /;
delta = -alpha;
z = x**2 + y**2;

* Bad
Set t / 1 * 100 /;
delta = - alpha;
z = x ** 2 + y ** 2;
```
- Operators used on sets

``` gams
* Good
K(t+1) = (1 - delta) * K(t) + I(t);

* Bad
K(t + 1) = (1 - delta) * K(t) + I(t);
```

### Data entry with / / ###

When entering information between `/`, leave a space around the data and before the first `/`. Keep the first `/` on the same line as the first data and the second `/` on the same line as the last data.

``` gams
* Good
Sets
  t / 1*10 /
  myset
    / element1
      element2 /
;

* Bad
Sets
  t/1*10/
  myset /
    element1
    element2
    /
;
```

### Extra spaces ###

Adding extra spaces is OK if it improves the alignment of `=` or `=e=`, and of identifiers and descriptive text.

``` gams
# Good
total = a + b + c;
mean  = (a + b + c) / n;
Sets
  products   "available production process"
  rawmateral "source of raw materials"
;

# Also fine
total = a + b + c;
mean = (a + b + c) / n;
Sets
  products "available production process"
  rawmateral "source of raw materials"
;
```

### Parentheses ###

Do not put spaces inside or outside parentheses, except for the above reasons.

``` gams
* Good
p_cpi = sum(i, a(i) * p(i)**(1 - sigma))**(1 / (1 - sigma));

** Bad
p_cpi = sum (i, a(i) * p(i)**(1 - sigma))**(1 / (1 - sigma) );
```

There is one exception: place a space before `()` when used with flow control operations `if`, `for`, `loop`, or `while`.

``` gams
# Good
for (i = 1 to 1000 by 10,
  display i;
);
if (x <= 0,
  y = 1;
elseIf (x > 0 and x < 1),
  y = 2;
else
  y = 3;
);

# Bad
for(i = 1 to 1000 by 10,
  display i;
);
if(x <= 0,
  y = 1;
elseIf(x > 0 and x < 1),
  y = 2;
else
  y = 3;
);
```

### Use at most one statement per line ###

A general rule is that there should not be 2 (or more) semicolons per line. Two exceptions are short flow control operations if they fit on one line and bounds setting for a given variable.

``` gams
* Good
y = 1;
z = 2;
for (i = 1 to 1000 by 10,
  z = z + i;
);
x.lo = -inf;
x.up = inf;

* Also fine
for (i = 1 to 1000 by 10, z = z + i;);
x.lo = -inf; x.up = inf;

* Bad
y = 1; z = 2;
```

## Blank lines ##

Leave a blank line after each declaration block (`Alias`, `Set`, `Parameter`, `Equation`, and `Model`) and after each equation (for space reasons, this recommendation is not followed in the other examples of this guide).

``` gams
* Good
Alias (i, j);

eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst));

z(i,j) = yes $ (not sameAs(i, j));

* Bad
Alias (i, j);
eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst));
z(i,j) = yes $ (not sameAs(i, j));
```

## Long lines ##

Strive to limit your code to 90 characters per line. This size fits comfortably on a printed page with a reasonably sized font or on half a laptop screen (which allows you to compare 2 files side by side).

_Other options:_

- 80 characters: a widespread default in many programming languages.
- 100 or 120 characters: this choice could reduce the need to add unnecessary line breaks for big models with long object names with the drawback of requiring more than half a screen on standard Full HD screens.

To break lines in equations, use the standard mathematical convention that the operator should start the new lines (optionally for the equality sign in equations).

``` gams
* Good
eq_p_cpi ..
  p_cpi =e=
  sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - subst))**(1 / (1 - subst)) $ CES
  + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;

* Also fine
eq_p_cpi ..
  p_cpi
  =e= sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - subst))**(1 / (1 - subst)) $ CES
  + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;

* Bad
eq_p_cpi ..
  p_cpi =e= sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - subst))**(1 / (1 - subst)) $ CES + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;
```

In case of long file calls (exceeding the maximum number of columns), break the call by putting the options in compile-time variables, in a [txt parameter file](https://www.gams.com/latest/docs/UG_GamsCall.html#UG_GamsCall_SecondaryParameterFile), or in a [YAML configuration file](https://www.gams.com/latest/docs/UG_GamsCall.html#UG_GamsCall_GAMSConfigYAML).

``` gams
* Good
$set options "LogOption=3 output=mygamsfile.lst GDX=mygdx"
$set ddparams "--method=3 --scen=0"
$call mygamsfile.gms %options% %ddparams%

* Bad
$call mygamsfile.gms LogOption=3 output=mygamsfile.lst GDX=mygdx --method=3 --scen=0
```

One exception is for object labels; it is OK to have long lines for labels since those are just informative text, not code.

Break lines in macros by ending each line with `\`.

``` gams
* Good
$macro laspeyres_price(SetSum, Price, Benchmark) \
  (sum(SetSum, Price * Benchmark) / sum(SetSum, Benchmark)) $ sum(SetSum, Benchmark)

* Bad
$macro laspeyres_price(SetSum, Price, Benchmark) (sum(SetSum, Price * Benchmark) / sum(SetSum, Benchmark)) $ sum(SetSum, Benchmark)
```

## Indentation ##

Do not use tabs! Use 2 spaces for indentation (options available in GAMS Studio in Settings/Editor & Log). Indent also dollar control options if needed, which can be done by using the symbols `$$`:

``` gams
* Good
Parameters
  a
  $$gdxLoad mygdx.gdx a
  b
;
$ifThen not %gams.logOption% == 3
  $$ifI %system.fileSys% == UNIX  $set nullFile > /dev/null
  $$ifI %system.fileSys% == MSNT  $set nullFile > nul
  $$if not set nullFile $abort %system.fileSys% not recognized
$else
  $$set nullFile
$endIf

* Bad
Parameters
  a
$gdxLoad mygdx.gdx a
b
;
$ifThen not %gams.logOption% == 3
$ifI %system.fileSys% == UNIX  $set nullFile > /dev/null
$ifI %system.fileSys% == MSNT  $set nullFile > nul
$if not set nullFile $abort %system.fileSys% not recognized
$else
$set nullFile
$endIf

* Also bad
$ifThen not %gams.logOption% == 3
$  ifI %system.fileSys% == UNIX  $set nullFile > /dev/null
$  ifI %system.fileSys% == MSNT  $set nullFile > nul
$  if not set nullFile $abort %system.fileSys% not recognized
$else
$  set nullFile
$endIf
```

### Nested indents ###

Use nested indentation if inside parentheses, and indent the closing parentheses at the same indentation level as that of the opening parentheses.

``` gams
* Good
eq_p ..
  p =e= sum((i,j),
    sh_g(i,j) * p_g(i,j)**(1 - subst)
  )**(1 / (1 - subst))
;

* Bad
eq_p ..
  p =e= sum((i,j),
  sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst))
;
eq_p ..
  p =e= sum((i,j),
    sh_g(i,j) * p_g(i,j)**(1 - subst)
           )**(1 / (1 - subst))
;
```

### Data entry with / / ###

If the data entry is too long to be kept on the same line as the declaration, use another line and indent by an additional level before the first `/`.

``` gams
* Good
Set
  mylongset "With an even longer label preventing the data entry on the line"
    / element1, element 2 /
;

* Bad
Set
  mylongset "With an even longer label preventing the data entry on the line"
  / element1, element 2 /
;

```

## Declaration statement ##

When declaring new identifiers (sets, parameters, variables, and equations), the keywords (`Set`, `Parameter`, etc) should be on a different line from the identifiers (except if there is only one identifier and its line does not exceed the maximum number of characters), each identifier shall be indented and on its line. The semicolon should be on its own, non-indented line.

Strive to always enter explanatory text for all objects.

Always specify object dimensions in declaration. Declare scalar parameters separately from indexed ones in a `Scalar` block statement.

``` gams
* Good
Sets
  i Countries
  k Goods
;
Parameter p(i) Price;
p(i) = 1;

* Bad
Sets
i Countries
k Goods;
Parameter p Price;
p(i) = 1;
```

### Pluralize ###

GAMS has singular and plural forms for all declarations (`Set` and `Sets`, `Parameter` and `Parameters`, `Variable` and `Variables`, etc). Use plural forms if declaring several identifiers.

### Universal set ###

Avoid using the universal set `(*)`, especially for data input and parameters and variables entering models. It is acceptable to use the universal set for reporting results.

### Careful use of $onMulti ###

`$onMulti` allows to declare several times the same symbol. This should be used only locally, for well-motivated cases, and always be followed by `$offMulti`.

## Parentheses ##

Refrain from using unbalanced parentheses in comments because doing so messes up with rainbow parentheses in code editors and parentheses matching.

``` gams
* Good
* (a) My section title

* Bad
* a) My section title
```

Do not use square brackets to replace parentheses; use only parentheses.

``` gams
* Good
p_cpi = sum(i, a(i) * p(i)**(1 - sigma))**(1 / (1 - sigma));

* Bad
p_cpi = sum(i, a(i) * p(i)**(1 - sigma))**[1 / (1 - sigma)];
```

## Naming ##

For large models for which it is not always possible to follow precisely the object names used in the model description, compose object names in two parts: a type and an explicit name separated by an underscore. The type should not necessarily follow GAMS object types (variables, parameters, ...) because the distinction between variables and parameters can be blurry depending on the simulations. It is better to use types that refer to what kind of objects are represented (prices, elasticities, shares, ...). One exception for the use of types is for sets. Since sets are used a lot, it is better to save space by not adding a type in front of their name. So, the only objects without a type are the sets.

Here is a suggestion of types for use in computable general and partial equilibrium models (complete the table to cover all types used in your model):

| type    | description                                           |
|---------|-------------------------------------------------------|
| `p`     | prices                                                |
| `v`     | values                                                |
| `q`     | quantities                                            |
| `sh`    | shares (something unit-less expected to sum to 1)     |
| `rt`    | ratios (something unit-less not expected to sum to 1) |
| `sig`   | substitution elasticities                             |
| `elast` | elasticities                                          |
| `inter` | intercepts of linear functions                        |
| `slope` | slopes of linear functions                            |
| `n`     | integers indicating numbers of elements               |
| `it`    | something used for iterating                          |
| `tx`    | tax rates                                             |
| `eq`    | equations                                             |
| `m`     | macros                                                |

It is possible to compose types when relevant, for example, for equations and macros:

``` gams
eq_p_cpi .. p_cpi =e= ...;
```

The explicit names should follow a standard casing convention in programming. Here, camelCase (with a lower-case first letter) is proposed for its conciseness and consistency with the default used for GAMS commands in GAMS Studio.

_Other option:_ snake\_case is slightly more readable than camelCase, but occupies more space, which can be an issue in big projects with many objects to name.

## Case of text ##

Even if GAMS is case-insensitive, use a consistent casing for GAMS commands and object names (see above for objects). For GAMS commands, the best option is to follow GAMS Studio auto-completion default: camelCase for most commands except declarations that follow BigCamelCase. Adopting GAMS Studio default will likely reduce coordination frictions inside teams and is a pretty readable option.

_Other options:_

- UPPERCASE if you like it when your code YELLS AT YOU.
- lowercase if you are too lazy to add a few upper-case letters here and there.

GAMS Studio allows to configure the default auto-completion (in Settings/Editor & Log) to camelcase, UPPERCASE, and lowercase. So, adjust the default to your choice. Do not mix the choice of cases, since this would be difficult to enforce inside a team.

``` gams
* Good
options limCol = 100;
Positive Variable p_cpi;
p_cpi.l = 1;

* Bad
OPTIONS limcol = 100;
positive variable p_cpi;
p_cpi.L = 1;
```
If you are worried that lower-case `l` could be confused with `1`, adopt a good programming font such as [Fira Code](https://github.com/tonsky/FiraCode) or [JetBrains Mono](https://www.jetbrains.com/fr-fr/lp/mono/).

## Semicolons ##

Place semicolons at the end of a code block without adding a space before it. If the code block spans several lines, place the semicolon on its line as the first character.

``` gams
* Good
eq_p ..
  p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst))
;

* Bad
eq_p ..
  p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - subst))**(1 / (1 - subst)) ;
```

## Quotes ##

Use `"`, not `'` for quoting text or set elements. The only exception is when the text already contains double quotes and no single quote.

## Comments ##

Use `#` for end-of-line comments for compatibility with Python and R, two languages with which GAMS is commonly used. Leave a space after `*` and `#`, and leave a space before the end-of-line comment.

Do not use `****` in comments, because it messes up with searching for infeasible equations that are indicated by the same characters string.

## File names ##

File names should be meaningful, end in `.gms` for files that can be compiled by GAMS (possibly following a restart), and end with `.inc` for files that cannot be compiled on themselves but must be called by other files. Avoid using special characters in file names, including spaces: stick with numbers, letters, -, and _.

## Platform-independent code ##

Strive to make your code platform independent:

- Use `/` to separate folders, not `\` which only works on Windows.
- Respect case of filenames (or avoid upper-case letters in filenames): filenames are case-insensitive on Windows by default but not on UNIX-based systems.
- Avoid using tools that works only on Windows. For example, to exchange data with Excel files use [GAMS Connect](https://www.gams.com/latest/docs/UG_GAMSCONNECT.html "har2gdx.exe my_harfile.har my_gdxfile.gdx") instead of `gdxxrw.exe`.
- If you cannot avoid using Windows-specific tools such as `har2gdx.exe`, test for the operating system and abort early.

``` gams
* Good
$include my_folder/my_file.inc
$if %system.fileSys% == UNIX $abort This code works only on Windows.
$call "har2gdx.exe my_harfile.har my_gdxfile.gdx"

* Bad
$include my_folder\my_file.inc
$call "har2gdx.exe my_harfile.har my_gdxfile.gdx"
```

## Don't Repeat Yourself (DRY) ##

The DRY principle applies to all programming languages. However, it can be difficult at the beginning to apply it in GAMS because of the lack of possibility to define functions. Use macros (`$macro`), `$include`, and `$batInclude` to remove repetitions from your code. Use comments to clarify what is done by these commands.

### Files called by `$batInclude` ###

Files called by `$batInclude` should have named arguments rather than the default names. Use `$setArgs` to define the names.

``` gams
* Good
$setArgs aa bb cc
x = %aa% - %bb% * %cc%;

* Bad
x = %1 - %2 * %3;
```

## License ##

This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1)
