# GAMS Style Guide

This file describes a comprehensive style guide for the [GAMS](https://www.gams.com/) (General Algebraic Modeling System) programming language. It provides a set of standards and best practices for writing clear, maintainable, and consistent code. It has been inspired by the [tidyverse style guide](https://style.tidyverse.org/) for the R programming language (with copied elements authorized by the associated [license](https://github.com/tidyverse/style/blob/main/LICENSE.md)).

This guide is necessarily opinionated in order to provide consistency. Feel free to adjust it to the style you want to adopt in your research group.

Other available style guides for GAMS:

- [MIT JPSPGC style guide](https://github.com/mit-jp/style-guides)
- [Good coding practices from GAMS documentation](https://www.gams.com/latest/docs/UG_GoodPractices.html), based on [Bruce McCarl's recommendations](https://forum.gams.com/uploads/short-url/6uUbctQlicqZdLM4PP1ZdQxhE2n.pdf)

**Author**: [Christophe Gouel](https://github.com/christophe-gouel)

## Spacing

Do not place spaces before commas and semicolons.

### Commas

Always put a space after a comma, except for the commas separating sets (outside the declaration of multiple sets on one line).

``` gams
* Good
Sets i, r;
Alias (i,j);
eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig));
z(i,j) = yes $ (not sameAs(i,j));
x = power(y, n);

* Bad
Sets i,r;
Alias (i, j);
eq_p .. p =e= sum((i, j),sh_g(i, j) * p_g(i, j)**(1 - sig))**(1 / (1 - sig));
z(i,j) = yes $ (not sameAs(i, j));
x = power(y,n);
```

### Infix operators

Most infix operators (`=`, `+`, `-`, `/`, `=e=`, etc.) should always be surrounded by spaces. This rule also applies to `$` used for conditional assignments and to the two dots `..` used for defining equations.

``` gams
* Good
eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig));
z(i,j) = yes $ (not sameAs(i,j));

* Bad
eq_p.. p=e=sum((i,j), sh_g(i,j)*p_g(i,j)**(1-sig))**(1/(1-sig));
z(i,j) = yes$(not sameAs(i,j));
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

### Data entry with / /

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

### Extra spaces

Adding extra spaces is OK if it improves the alignment of `=` or `=e=`, and of identifiers and descriptive text.

``` gams
* Good
total = a + b + c;
mean  = (a + b + c) / n;
Sets
  products   "available production process"
  rawmateral "source of raw materials"
;

* Also fine
total = a + b + c;
mean = (a + b + c) / n;
Sets
  products "available production process"
  rawmateral "source of raw materials"
;
```

### Parentheses

Do not put spaces inside or outside parentheses, except for the above reasons.

``` gams
* Good
p_cpi = sum(i, a(i) * p(i)**(1 - sig))**(1 / (1 - sig));

** Bad
p_cpi = sum (i, a(i) * p(i)**(1 - sig))**(1 / (1 - sig) );
```

There is one exception: place a space before `()` when used with flow control operations `if`, `for`, `loop`, or `while`.

``` gams
* Good
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

* Bad
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

### Use at most one statement per line

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

## Blank lines

Leave a blank line after each declaration block (`Alias`, `Set`, `Parameter`, `Equation`, and `Model`) and after each equation (for space reasons, this recommendation is not followed in the other examples of this guide).

``` gams
* Good
Alias (i,j);

eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig));

z(i,j) = yes $ (not sameAs(i,j));

* Bad
Alias (i,j);
eq_p .. p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig));
z(i,j) = yes $ (not sameAs(i,j));
```

## Long lines

Strive to limit your code to 90 characters per line. This size fits comfortably on  half a laptop screen with a reasonably sized font (which allows you to compare 2 files side by side or to have a running process along your code).

_Other options:_

- 80 characters: a widespread default in many programming languages.
- 100 or 120 characters: this choice could reduce the need to add unnecessary line breaks for big models with long object names with the drawback of requiring more than half a screen on standard Full HD screens.

To break lines in equations, use the standard mathematical convention that the operator should start the new lines (optionally for the equality sign in equations).

``` gams
* Good
eq_p_cpi ..
  p_cpi =e=
  sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - sig))**(1 / (1 - sig)) $ CES
  + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;

* Also fine
eq_p_cpi ..
  p_cpi
  =e= sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - sig))**(1 / (1 - sig)) $ CES
  + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;

* Bad
eq_p_cpi ..
  p_cpi =e= sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - sig))**(1 / (1 - sig)) $ CES + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;
```

Note that there is a fundamental difference between indenting after `=` in an assignment and after `=e=` (and other equation types) in an equation. Since objects can be moved around `=e=` (`supply =e= demand` is equivalent to `supply - demand =e= 0`), there is no need for an additional indentation level after `=e=`. Here is the same example as above but as an assignment rather than an equation.

``` gams
* Good
p_cpi =
  sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - sig))**(1 / (1 - sig)) $ CES
  + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;

* Also fine
p_cpi = sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - sig))**(1 / (1 - sig)) $ CES
      + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas
;

* Bad
p_cpi = sum((i,j), sh_good(i,j) * p_good(i,j)**(1 - sig))**(1 / (1 - sig)) $ CES + prod((i,j), p_good(i,j)**sh_good(i,j)) $ CobbDouglas;
```

In case of long file calls (exceeding the maximum number of columns), break the call by putting the options in compile-time variables, in a [txt parameter file](https://www.gams.com/latest/docs/UG_GamsCall.html#UG_GamsCall_SecondaryParameterFile), or in a [YAML configuration file](https://www.gams.com/latest/docs/UG_GamsCall.html#UG_GamsCall_GAMSConfigYAML).

``` gams
* Good
$set options "LogOption=3 limCol=0 limRow=0 output=my_gams_file.lst GDX=my_gdx"
$set ddparams "--method=3 --scen=0"
$call gams my_gams_file.gms %options% %ddparams%

* Bad
$call gams my_gams_file.gms LogOption=3 limCol=0 limRow=0 output=my_gams_file.lst GDX=my_gdx --method=3 --scen=0
```

One exception is for object labels; it is OK to have long lines for labels since those are just informative text, not code.

Break lines in macros by ending each line with `\`.

``` gams
* Good
$macro m_p_laspeyres(setSum, price, benchmark) \
  (sum(setSum, price * benchmark) / sum(setSum, benchmark)) $ sum(setSum, benchmark)

* Bad
$macro m_p_laspeyres(setSum, price, benchmark) (sum(setSum, price * benchmark) / sum(setSum, benchmark)) $ sum(setSum, benchmark)
```

## Indentation

Do not use tabs! Use 2 spaces for indentation (options available in GAMS Studio in Settings/Editor & Log). Indent also dollar control options if needed, which can be done by using the symbols `$$`:

``` gams
* Good
Parameters
  a
  $$gdxLoad my_gdx.gdx a
  b
;

* Bad
Parameters
  a
$gdxLoad mygdx.gdx a
b
;
```

### Dollar control options

Indentation with dollar control options can be tricky, so indentation rules have to be adapted for readability and consistency with respect to the surrounding code. In the first example, standard indentation is applied.

``` gams
* Good
$ifThen exist my_other_gdx.gdx
  $$gdxLoad my_other_gdx.gdx b
$else
  b = 1;
$endIf

* Bad
$ifThen exist my_other_gdx.gdx
$$gdxLoad my_other_gdx.gdx b
$else
b = 1;
$endIf

* Better but also bad
$ifThen exist my_other_gdx.gdx
$  gdxLoad my_other_gdx.gdx b
$else
  b = 1;
$endIf
```

In the second example, it is better to keep the surrounding indentation.

``` gams
* Good
Model mymodel
  / eq_a
    eq_b
$ifThen %add_eq%
    eq_c
$endIf
    Eq_d /
;

* Bad
Model mymodel
  / eq_a
    eq_b
$ifThen %add_eq%
      eq_c
$endIf
    eq_d /
;
```

### Nested indents

Use nested indentation if inside parentheses, and indent the closing parentheses at the same indentation level as that of the opening parentheses.

``` gams
* Good
eq_p ..
  p =e= sum((i,j),
    sh_g(i,j) * p_g(i,j)**(1 - sig)
  )**(1 / (1 - sig))
;

* Bad
eq_p ..
  p =e= sum((i,j),
  sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig))
;

* Also bad
eq_p ..
  p =e= sum((i,j),
    sh_g(i,j) * p_g(i,j)**(1 - sig)
           )**(1 / (1 - sig))
;
```

### Data entry with / /

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

## Declaration statement

When declaring new identifiers (sets, parameters, variables, and equations), the keywords (`Set`, `Parameter`, etc.) should be on a different line from the identifiers (except if there is only one identifier and its line does not exceed the maximum number of characters), each identifier shall be indented and on its line. The semicolon should be on its own, non-indented line.

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

### Pluralize

GAMS has singular and plural forms for all declarations (`Set` and `Sets`, `Parameter` and `Parameters`, `Variable` and `Variables`, etc.). Use plural forms if declaring several identifiers.

### Careful use of `$onMulti`

`$onMulti` allows to declare several times the same symbol. This should be used only locally, for well-motivated cases, and always be followed by `$offMulti`.

## Domain checking

To avoid data problems, rely on GAMS [domain checking](https://www.gams.com/latest/docs/UG_SetDefinition.html#INDEX_domain_21_checking). It is automatic, but two cases require attention.

### Universal set

Avoid using the universal set `(*)`, especially for data input and parameters and variables entering models. It is acceptable to use the universal set for reporting results.

### Loading from GDX files

When loading data from a GDX file, the `$load` command automatically filters domain violations without raising a compilation error. To avoid this behavior, consider using instead one of the `$loadDC*` commands or activate domain checking for `$load` using `$offFiltered`.

If the filtering of domain violations is the intended behavior, use `$loadFiltered` for clarity.

## Parentheses

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
p_cpi = sum(i, a(i) * p(i)**(1 - sig))**(1 / (1 - sig));

* Bad
p_cpi = sum(i, a(i) * p(i)**(1 - sig))**[1 / (1 - sig)];
```

## Naming

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
| `tr`    | tax rates                                             |
| `tp`    | tax power (i.e., 1 + tax rate)                        |
| `pt`    | aftex-tax prices                                      |
| `eq`    | equations                                             |
| `m`     | macros                                                |

It is possible to compose types when relevant, for example, for equations and macros:

``` gams
eq_p_cpi .. p_cpi =e= ...;
```

For macros, the `m` is not always necessary. If a macro is just used to play the role of a variable in an equation, it is OK to label it as the corresponding object.

The explicit names should follow a standard casing convention in programming. Here, camelCase (with a lower-case first letter) is proposed for its conciseness and consistency with the default used for GAMS commands in GAMS Studio.

_Other option:_ snake\_case is slightly more readable than camelCase, but occupies more space, which can be an issue in big projects with many objects to name.

``` gams
* Good
eq_pt_finalGood(s) .. pt_finalGood(s) =e= tp_finalGood(s) * p_finalGood(s);

* Bad
eq_pricegoodaftertax(s) .. pricefinalgoodaftertax(s) =e= (1 + taxfinalgood(s)) * pricefinalgood(s);
```

In square models (`cns`), strive to associate the name of each equation to the variable it determines. This helps make sure that each variable is associated to an equation.

## Case of text

Even if GAMS is case-insensitive, use a consistent casing for GAMS commands and object names (see above for objects). For GAMS commands, the best option is to follow GAMS Studio auto-completion default: camelCase for most commands except declarations that follow PascalCase. Adopting GAMS Studio default will likely reduce coordination frictions inside teams and is a pretty readable option.

_Other options:_

- UPPERCASE if you like it when your code YELLS AT YOU.
- lowercase if you are too lazy to add a few upper-case letters here and there.

GAMS Studio allows to configure the default auto-completion (in Settings/Editor & Log) to camelCase, UPPERCASE, and lowercase. So, adjust the default to your choice. Do not mix the choice of cases, since this would be difficult to enforce inside a team.

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

## Semicolons

Place semicolons at the end of a code block without adding a space before it. If the code block spans several lines, place the semicolon on its line as the first character.

``` gams
* Good
eq_p ..
  p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig))
;

* Bad
eq_p ..
  p =e= sum((i,j), sh_g(i,j) * p_g(i,j)**(1 - sig))**(1 / (1 - sig));
```

The semicolon at the end of a statement can be omitted if a new GAMS keyword follows. Do not use this option because the resulting code would be less readable, and errors could be introduced if a different type of statement replaces the keyword.

## Quotes

Use `"`, not `'` for quoting text or set elements. The only exception is when the text already contains double quotes and no single quote.

## Comments

Use `#` for end-of-line comments for compatibility with Python and R, two languages with which GAMS is commonly used. Leave a space after `*` and `#`, and leave a space before the end-of-line comment.

Do not use `****` in comments because it messes up with searching for infeasible equations that are indicated by the same character string.

## File names

File names should be meaningful, end in `.gms` for files that can be compiled by GAMS (possibly following a restart), and end with `.inc` for files that cannot be compiled on themselves but must be called by other files. Avoid using special characters in file names, including spaces: stick with numbers, letters, -, and _.

For filenames, privilege snake\_case and kebab-case to camelCase and PascalCase to avoid problems with case-insensitive file systems (e.g., macOS or Microsoft Windows).

## Platform-independent code

Strive to make your code platform-independent:

- Use `/` to separate folders, not `\` which only works on Microsoft Windows.
- Respect the case of filenames (or avoid upper-case letters in filenames): filenames are case-insensitive on macOS and Microsoft Windows by default but not on Linux.
- Avoid using tools that work only on Microsoft Windows. For example, to exchange data with Excel files, use [GAMS Connect](https://www.gams.com/latest/docs/UG_GAMSCONNECT.html "har2gdx.exe my_harfile.har my_gdxfile.gdx") instead of `gdxxrw.exe`.
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

## Logical conditions

Be consistent in logical conditions: use either operators (`<=`, `<>`, etc.) or text (`le`, `ne`, etc.), but avoid mixing them. Since operators are more quickly readable, they are a better default.

## Code organization

Proper code organization is pivotal to the maintainability, readability, and scalability of GAMS projects.

### Organizing code into separate files

Separating GAMS code into multiple files serves several practical purposes:

- **Modularity**: While not expanded upon here, modularity is crucial to many large-scale modeling projects.
- **Operational Efficiency**: Distinct phases of the modeling process, such as simulation runs and result extraction, benefit from being in separate files. For instance, after running simulations, you might need to compute new indicators to interpret the results more clearly. If simulations and result processing are in separate files, you can modify the latter without rerunning the former. This is also relevant for dynamic models with time-consuming baseline simulations for which the baseline simulations should be separated from the counterfactual simulations. This separation can be done with either of the following two approaches:
    - At the end of the first GAMS file, dump all or all relevant objects in a GDX file (using `execute_unload` or a command line option), and in the second GAMS file load the GDX (you can declare all objects and import them with `$declareAndLoad`).
    - Utilize the command line options [save and restart](https://www.gams.com/latest/docs/UG_SaveRestart.html) to manage state continuity across different running stages.
- **Brevity and Clarity**: Lengthy script files can be overwhelming and challenging to navigate. Grouping related code segments into smaller, focused files that can be included in a master file through `$include` commands helps avoid monolithic and unwieldy script files.


### Adhering to the DRY principle

"Don't Repeat Yourself" (DRY) is a fundamental coding principle applicable across all programming languages to reduce redundancy. While GAMS doesn't support traditional function definitions, there are other methods to achieve DRY:

- Employ **macros** (`$macro`), `$include`, and `$batInclude` to eliminate code repetition.
- Accompany these commands with **comments** to elucidate their purpose; such commands can become obscure without proper documentation.

To demonstrate applying the DRY principle in GAMS, consider the following:

1. **Use macros**: Define macros for repetitive tasks or calculations.
    ``` gams
    * Calculate a Laspeyres price index
    $macro m_p_laspeyres(setSum, price, benchmark) \
      (sum(setSum, price * benchmark) / sum(setSum, benchmark)) $ sum(setSum, benchmark)
    ```
2. **Include files**: Keep common sets, parameters, and scalars in a separate file to `$include` where needed.
    ``` gams
    * Declare sets
    $include "common_sets.gms"
    ```
3. **Batch includes**: Utilize `$batInclude` for repeating a sequence of commands across multiple contexts or scenarios.
    ``` gams
    * Setup scenario 1
    $batInclude "scenario_setup.gms" scenario1
    ```

### Files called by `$batInclude`

Files called by `$batInclude` should have named arguments rather than the default names. Use `$setArgs` to define the names.

``` gams
* Good
$setArgs aa bb cc
x = %aa% - %bb% * %cc%;

* Bad
x = %1 - %2 * %3;
```

### Code options

Aim to gather most code options and switches at the top of the model file. This includes dollar control options (e.g., `$onFiltered`), compile-time variables defined by `$set*` commands, and standard options defined by the `option` statement. For options that could be unclear for the other users, add a small comment. Use a separate line for each option.

``` gams
* Good
$eolCom #
$offFiltered # Ensure domain checking for all gdx load operations

options
  limRow = 0
  limCol = 0
  solveLink = 5 # Pass model instance in-memory
;

* Bad
$offFiltered eolCom #

option limRow = 0, limCol = 0, solveLink = 5;
```

## Tests

Unlike many other programming languages, GAMS programs are less conducive to unit testing because models are typically intricate and can't be easily decomposed into smaller, testable units. Despite this challenge, testing parts of the model—especially those related to calibration and specific model properties—is feasible and crucial.

### Essential tests for GAMS models

To ensure the reliability and accuracy of GAMS models, consider implementing the following tests:

- **Summation check**: Ensure all shares add up to 1.
- **Equilibrium confirmation**: For models calibrated against an equilibrium, check that this equilibrium condition is satisfied using a solve with zero iteration (`option iterLim = 0;`).
- **General equilibrium model tests**:
    - Verify **Walras' law**, which states that the value of excess demand must be zero in an economy. For this, verify that the equation dropped from the model still holds after a shock.
    - Test for **homogeneity of degree zero in prices** to confirm that a change in the numeraire does not affect real variables by changing the value of the numeraire.
    - Assess **real homogeneity** as applicable, which pertains to the model's behavior when all factors are scaled up by the same proportion: this change should not lead to any change in relative prices in the absence of fixed costs and non-homothetic demand functions.
- **Baseline confirmation test**: For dynamic models, a no-shock simulation should recover the same dynamic path as simulated in the baseline.
- **Robustness to aggregation**: Run the above tests and simple simulations with different databases by varying the number of sectors and countries. This ensures that sparsity issues (e.g., sectors with 0 production) are handle properly in the code.
- **Results replication**: Define a reference counterfactual simulation and verify that its results are replicated after a change of code (provided that this code change was not intended to affect the results).

### Automating tests with continuous integration

Leveraging continuous integration tools enables the automation of these tests, ensuring they are executed upon each new commit. A practical guide to using continuous integration with GAMS is available in this [blog post](https://www.gams.com/blog/2023/08/modern-gams-teaching-gams-with-github-classroom/), which illustrates how to apply these principles for student models. This automated approach is also scalable to larger models.

With the [GAMS Docker image](https://hub.docker.com/r/gams/gams), it's possible to spin up a new GAMS instance for each commit. This allows for the execution of tests on a simpler model variant, providing immediate feedback regarding the model’s expected behavior. By incorporating this into your testing workflow, you can significantly enhance the maintainability and quality of your GAMS programs.

## License

This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1)
