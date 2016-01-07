# Number Component

<!-- toc -->

## Availability

Orbeon Forms 4.0.

## What it does

The number component is designed to enter integer or decimal numbers. See [Currency](../../form-runner/component/currency.md) for a similar component designed to enter currency amounts.

![](images/xbl-number.png)

## Basic usage

You use the number component like a regular input field, for example:

```xml
<fr:number ref="my-number">
  <xf:label>Quantity</xf:label>
</fr:number>
```

## Parameters

`fr:number` supports parameters, which you can set via properties or directly on fr:number:

* `prefix`: optional prefix shown before the number
* `suffix`: optional suffix shown after the number
* `digits-after-decimal`: digits to show after the decimal point (by default 0, which means the number is an integer)
* `decimal-separator`: single character to use as decimal separator
* `grouping-separator`: single character to use as thousands separator separator (can be blank)
* `round-when-formatting`: when formatting the number for display, whether to round the value to `digits-after-decimal` if there are more digits after the decimal point or not. The default is `false`.
    * SINCE Orbeon Forms 4.11
    * Rounding uses the [half to even](https://en.wikipedia.org/wiki/Rounding#Round_half_to_even) method.
* `round-when-storing`: when storing the number entered by the user, whether to round the value to `digits-after-decimal` if there are more digits after the decimal point or not. The default is `false`.
    * SINCE Orbeon Forms 4.11
    * Rounding uses the [half to even](https://en.wikipedia.org/wiki/Rounding#Round_half_to_even) method.

These are the default values of the properties:

```xml
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.prefix"
    value=""/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.suffix"
    value=""/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.digits-after-decimal"
    value="0"/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.decimal-separator"
    value="."/>
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.grouping-separator"
    value=","/>
<property 
    as="xs:boolean" 
    name="oxf.xforms.xbl.fr.number.round-when-formatting"         
    value="false"/>
<property 
    as="xs:boolean" 
    name="oxf.xforms.xbl.fr.number.round-when-storing"            
    value="false"/>
```

## Examples

### Use of a suffix

Here is an example with a suffix:

![](images/xbl-number.png)

```xml
<fr:number ref="my-number" suffix="m/s">
    <xf:label>Number</xf:label>
    <xf:hint>Number field with validation</xf:hint>
</fr:number>
```

### Rounding

[SINCE Orbeon Forms 4.11]

#### Rouding when formatting only

Assume the following properties or corresponding attributes:

```xml
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.digits-after-decimal"
    value="2"/>
<property 
    as="xs:boolean" 
    name="oxf.xforms.xbl.fr.number.round-when-formatting"         
    value="true"/>
<property 
    as="xs:boolean" 
    name="oxf.xforms.xbl.fr.number.round-when-storing"            
    value="false"/>
```


Scenario:

- User types "12345.678".
- Because `round-when-storing` is `false`, the value is not rounded and stored as is in the data.
- When the user focuses out of the field, the value is formatted and rounded, because `round-when-formatting` is set to `true`.
- Field displays "12,345.68".

This is useful if you want to keep the full precision of decimal values in the data, but format them nicely to a given number of decimals.

#### Rouding when formatting and when storing

Assume the following properties or corresponding attributes:

```xml
<property
    as="xs:string"
    name="oxf.xforms.xbl.fr.number.digits-after-decimal"
    value="2"/>
<property 
    as="xs:boolean" 
    name="oxf.xforms.xbl.fr.number.round-when-formatting"         
    value="true"/>
<property 
    as="xs:boolean" 
    name="oxf.xforms.xbl.fr.number.round-when-storing"            
    value="true"/>
```


Scenario:

- User types "12345.678".
- Because `round-when-storing` is `true`, the value is rounded and stored as "12345.68" in the data.
- When the user focuses out of the field, the value is formatted and rounded, because `round-when-formatting` is set to `true`.
- Field displays "12,345.68".

*NOTE: Here rounding during formatting doesn't change the value since it was rounded when stored in the first place.*


This is useful if you don't want to keep the full precision of decimal values in the data and want to force a maximum number of digits after the decimal point.
