# Validation functions

<!-- toc -->

## Introduction

These functions are typically used on the `constraint` attribute of the `xf:bind` element. For example, as used by Form Builder:

```xml
<xf:bind
    id="control-1-bind"
    name="control-1"
    ref="control-1"
    type="xf:decimal"
    constraint="xxf:fraction-digits(2)"/>
```

## xxf:fraction-digits()

[SINCE Orbeon Forms 2016.1]

```xpath
xxf:fraction-digits($digits as xs:integer?) as xs:boolean
```

Return `true()` if the context item converted to a string via the `string()` function contains at most the number of consecutive digits
specified by `$digits` after the decimal point `.`, or if there is no decimal point. Also return `true()` if `$digits` is the empty sequence.

Note the following:

- Trailing `0`s are not counted.
- Characters other than digits are ignored.

For example, if the context item is the string "abc123.d450f0g", the resulting consecutive digits after the decimal point considered are "45" and the number of consecutive digits
after the decimal point is 2.

## xxf:max-length()

[SINCE Orbeon Forms 4.10]

```xpath
xxf:max-length($max as xs:integer?) as xs:boolean
```

Return `true()` if the context item converted to a string via the `string()` function contains at most the number of characters
specified by `$max`. Also return `true()` if `$max` is the empty sequence.

Following [XPath 2.0](http://www.w3.org/TR/xpath-functions/#string-types):

> what is counted is the number of XML characters in the string (or equivalently, the number of Unicode code points). Some implementations may represent a code point above xFFFF using two 16-bit values known as a surrogate. A surrogate counts as one character, not two.

## xxf:min-length()

[SINCE Orbeon Forms 4.10]

```xpath
xxf:min-length($min as xs:integer?) as xs:boolean
```

Return `true()` if the context item converted to a string via the `string()` function contains at least the number of characters
specified by `$min`. Also return `true()` if `$min` is the empty sequence.

Following [XPath 2.0](http://www.w3.org/TR/xpath-functions/#string-types):

> what is counted is the number of XML characters in the string (or equivalently, the number of Unicode code points). Some implementations may represent a code point above xFFFF using two 16-bit values known as a surrogate. A surrogate counts as one character, not two.

## xxf:negative()

[SINCE Orbeon Forms 2016.1]

```xpath
xxf:negative() as xs:boolean
```

- return `true()` if the context item converted to a string via the `string()` function parses as an `xs:decimal` which is negative
- return `false()` otherwise

## xxf:non-negative()

[SINCE Orbeon Forms 2016.1]

```xpath
xxf:non-negative() as xs:boolean
```

- return `true()` if the context item converted to a string via the `string()` function parses as an `xs:decimal` which is not negative
- return `false()` otherwise

## xxf:non-positive()

[SINCE Orbeon Forms 2016.1]

```xpath
xxf:non-positive() as xs:boolean
```

- return `true()` if the context item converted to a string via the `string()` function parses as an `xs:decimal` which is not positive
- return `false()` otherwise

## xxf:positive()

[SINCE Orbeon Forms 2016.1]

```xpath
xxf:positive() as xs:boolean
```

- return `true()` if the context item converted to a string via the `string()` function parses as an `xs:decimal` which is positive
- return `false()` otherwise
