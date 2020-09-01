---
title: "Representation of Data"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:32+10:00
tileColour: "#8BBEE8"
---


## Representation of Data

{{% section %}}

As digital circuits (i.e. computers) only deal with binary data (1s and 0s) - there exists different _encoding schemas_ to represent different data types.

---

### ASCII

ASCII is a representation of the common characters used throughout the world (i.e the keys on your keyboard)

![](https://www.asciitable.com/index/asciifull.gif)

---

### Positive Integers

Positive integers are represented as their binary representation - bound to the data type assigned

i.e. the byte `81 == 0b01010001`

---

### Negative Integers

Negative integers are represented as the two's complement of their binary representation.

> \- Two's Complement -  
Invert the bits, and add `1`.

```
(byte)  81 ==  0b01010001
(byte) -81 == ~0b01010001 + 1
           ==  0b10101110
                      + 1
           ==  0b10101111     <--- Two's complement of 81
```

---

### Negative Integers

Given the representation of the byte `81`

* (byte) `81 == 0b01010001`
* (byte) `-81 == 0b10101111`

We can verify two's complement works by adding both binary numbers together (ie `81 + -81 = 0`)

(byte) `0b01010001 + 0b10101111 = 0`

---

### Floating Point Numbers

Floating Point numbers (i.e. 103.121369126) are represented by the `IEEE 754` standard.  

The decimal part of a floating point number is represented as the sum of negative powers of 2

i.e. 0.75 = $\frac{1}{2}$ + $\frac{1}{4}$

---

### Floating Point Numbers

There exists a single and double precision variant.

|Single Precision|Double Precision|
|:--------------:|:--------------:|
|First bit - sign<br/>Next 8 bits - exponent<br/>Next 23 bits - fraction|First bit - sign<br/>Next 11 bits - exponent<br/>Next 52 bits - fraction|

---

### Floating Point Numbers

#### Floating Point to IEEE 754

1) Convert the floating point number into binary, keeping the decimal  
2) Turn the floating point number into its scientific form (one digit on the left of the decimal)  
3) Convert the exponent into its binary representation and add it to a bias value ($ 2^n -1 $)  
4) Answer is (sign)(exponent+bias)(fraction)

---

### Floating Point Numbers

#### IEEE 754 to Floating Point

1) Note the sign of the value  
2) Subtract the bias ($ 2^n -1 $) from the exponent  
3) Assemble the floating point 1.(fraction)  
4) Shift the decimal place by the value from (2)

{{% /section %}}
