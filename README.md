# sass Modules fail to load - when you use the wrong file extension like I did..

Take care with the file extension. Late at night it's easy to get it wrong.

# All the below is wrong as I had .sccs instead of scss

The Sass documentation describes the @use syntax as a way to split up code into modules.

https://sass-lang.com/guide/#modules

This repo has copied the example sccs code snippets and stored them in:
- sccs/_base.sccs
- sccs/style.sccs

On windows the @use statement causes an error. 

All attempts to get sass run on sccs/style.sccs have failed with:
```
Error: Can't find stylesheet to import.
  ╷
1 │ @use 'base';
  │ ^^^^^^^^^^^
  ╵
  sccs\style.sccs 1:1  root stylesheet
```



Test environment:
- Window 11.
- Windows Cmd.exe prompt
- sass V 1.66.1

## setup

```
choco install sass

C:\dev\sccs\sass_use_error>sass --version
1.66.1

```


## Tests

Setup: This repo was cloned to a local directory of:
```
 C:\dev\sccs\sass_use_error
```

1.  Expected error as no --load-path

```
C:\dev\sccs\sass_use_error>sass sccs\style.sccs:css\style.css
Error: Can't find stylesheet to import.
  ╷
2 │ @use 'base';
  │ ^^^^^^^^^^^
  ╵
  sccs\style.sccs 2:1  root stylesheet

```

2.  Load path using relative references

```
C:\dev\sccs\sass_use_error>sass --trace --load-path=sccs sccs\style.sccs:css\style.css
Error: Can't find stylesheet to import.
  ╷
2 │ @use 'base';
  │ ^^^^^^^^^^^
  ╵
  sccs\style.sccs 2:1  root stylesheet

package:sass/src/visitor/evaluate.dart 1662                       _EvaluateVisitor._loadStylesheet
package:sass/src/visitor/evaluate.dart 633                        _EvaluateVisitor._loadModule.<fn>
package:sass/src/visitor/evaluate.dart 3362                       _EvaluateVisitor._withStackFrame
package:sass/src/visitor/evaluate.dart 631                        _EvaluateVisitor._loadModule
package:sass/src/visitor/evaluate.dart 2140                       _EvaluateVisitor.visitUseRule
package:sass/src/ast/sass/statement/use_rule.dart 62              UseRule.accept
package:sass/src/visitor/evaluate.dart 939                        _EvaluateVisitor.visitStylesheet
package:sass/src/visitor/evaluate.dart 747                        _EvaluateVisitor._execute.<fn>
package:sass/src/visitor/evaluate.dart 3135                       _EvaluateVisitor._withEnvironment
package:sass/src/visitor/evaluate.dart 716                        _EvaluateVisitor._execute
package:sass/src/visitor/evaluate.dart 545                        _EvaluateVisitor.run.<fn>.<fn>
package:sass/src/visitor/evaluate.dart 3471                       _EvaluateVisitor._addExceptionTrace
package:sass/src/visitor/evaluate.dart 545                        _EvaluateVisitor.run.<fn>
dart:async                                                        runZoned
package:sass/src/evaluation_context.dart 66                       withEvaluationContext
package:sass/src/visitor/evaluate.dart 539                        _EvaluateVisitor.run
package:sass/src/visitor/evaluate.dart 102                        evaluate
package:sass/src/compile.dart 163                                 _compileStylesheet
package:sass/src/compile.dart 75                                  compile
package:sass/src/executable/compile_stylesheet.dart 103           compileStylesheet
c:\programdata\chocolatey\lib\sass\tools\source\bin\sass.dart 89  main


```

3. All following variations of load path result in the same *Error: Can't find stylesheet to import* failure:

```
cd C:\dev\sccs\sass_use_error


sass --load-path=sccs sccs\style.sccs:css\style.css
sass --load-path=sccs\ sccs\style.sccs:css\style.css
sass --load-path=.\sccs sccs\style.sccs:css\style.css
sass --load-path=.\sccs\ sccs\style.sccs:css\style.css
sass --load-path=. sccs\style.sccs:css\style.css
sass --load-path=.\ sccs\style.sccs:css\style.css
sass --load-path=C:\dev\sccs\sass_use_error\sccs sccs\style.sccs:css\style.css
sass --load-path=C:\dev\sccs\sass_use_error\sccs\ sccs\style.sccs:css\style.css
sass --load-path="C:\dev\sccs\sass_use_error\sccs" sccs\style.sccs:css\style.css

sass --load-path="sccs/\" sccs\style.sccs:css\style.css


sass --load-path="sccs/" sccs\style.sccs:css\style.css
sass --load-path="sccs//" sccs\style.sccs:css\style.css

sass --load-path=./sccs sccs\style.sccs:css\style.css
sass --load-path="./sccs" sccs\style.sccs:css\style.css
sass --load-path=.//sccs sccs\style.sccs:css\style.css
sass --load-path=".//sccs" sccs\style.sccs:css\style.css

-- this variation results in no error at all
sass --load-path="sccs\" sccs\style.sccs:css\style.css


```