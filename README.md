# `RexExp.escape` Proposal

Proposal for adding a `RegExp.escape` method to the ECMAScript standard.

## Status

This proposal is a [stage 0 (strawman) proposal](https://docs.google.com/document/d/1QbEE0BsO4lvl7NFTn5WXWeiEIBfaVUF7Dk0hpPpPDzU/edit#) and is awaiting specification, implementation and input.

## Motivation

See [this issue](https://esdiscuss.org/topic/regexp-escape). It is often the case when we want to build a regular expression out of a string without treating special characters from the string as special regular expression tokens. For example if we want to replace all occurrences of the the string `Hello.` which we got from the user we might be tempted to do `ourLongText.replace(new RegExp(text, "g"))` but this would match `.` against any character rather than a dot.

This is a fairly common use in regular expressions and standardizing it would be useful. 

In other languages: 

 - Perl: quotemeta(str) - see [the docs](http://perldoc.perl.org/functions/quotemeta.html)
 - PHP: preg_quote(str) - see [the docs](http://php.net/manual/en/function.preg-quote.php)
 - Python: re.escape(str) - see [the docs](https://docs.python.org/2/library/re.html#re.escape)
 - Ruby: Regexp.escape(str) - see [the docs](http://ruby-doc.org/core-2.2.0/Regexp.html#method-c-escape)
 - Java: Pattern.quote(str) - see [the docs](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html#quote(java.lang.String))
 - C#, VB.NET: Regex.Escape(str) - see [the docs](https://msdn.microsoft.com/en-us/library/system.text.regularexpressions.regex.escape(v=vs.110).aspx)

Note that the languages differ in what they do - (perl does something different from C#) but they all have the same goal. 

## Proposed Solution

We propose the addition of an `RegExp.escape` function, such that strings can be escaped in order to be used inside regular expressions:

```js
var str = prompt("Please enter a string");
str = RegExp.escape(str);
alert(ourLongText.replace(new RegExp(str, "g")); // handles reg exp special tokens with the replacement.
```

There is initial previous work here: https://gist.github.com/kangax/9698100 we'll base our proposal on top of that one. 

##FAQ

##Semantics

### RegExp.escape(S)

When the **escape** function is called with an argument _S_ the following steps are taken:

1. let **str** be [ToString](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-tostring)(S).
2. [ReturnIfAbrupt](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-returnifabrupt)(str).
3. Let **cpList** be a [List](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-list-and-record-specification-type) containing in order the code points as defined in [6.1.4](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-ecmascript-language-types-string-type) of **str**, starting at the first element of **str**.
4. let **cuList** be a new [List](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-list-and-record-specification-type)
5. For each code point **c** in **cpList** in List order, do:
 1. If **c** is a [SyntaxCharacter](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-patterns) then do:
   1. Append **"\"** to **cuList**.
 2. Append **c** to  **cpList**.
6. Let **L** be a String whose elements are, in order, the elements of **cuList**.
7. Return **L**.

##Usage Examples
