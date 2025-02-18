---
title: Verify IBAN
categories: [haskell, modulo]
tags: [function]     # TAG names should always be lowercase
math: true
---

# Aim 
The aim is to write a function `ibanok`, which takes a string as argument, containing an IBAN and returning whether this number is valid or not, and the expected checksum. 

# How it works - an example
An IBAN has the following form:
* CHxx 0900 0000 3002 3128 4

The  check sum (xx) is calculated as follows:
* Take 0900 0000 3002 3128 4 CH00.
Then encode the CH with numbers:
A=10, B=11, C=12, and so on. 
Take the whole as number and devide modulo 97.
The checksum is then 98 minus this number. 

# To do
* what is the type of the function to implement?
* Then define the function. ,
* You can assume, the given string containing the IBAN, is valid, means two letters (capital or not) followed by digits.
* Rewrite the IBAN as 
$$\sum^{n}_{k=0} 100^k \;a_k$$
knowing $100 \equiv 3 (mod \;97)$, instead of just using the `mod` function on the very big IBAN. 


# Haskell implementation
```haskell
ibanok :: [char] -> (Bool, Int)
ibanok = todo
```

## How to Test
Test the function using some known IBANs.  
```bash
Prelude> ibanok "ch93 0900 0000 3211 1234 4"
(False, 16)
```
means CH16 0900 0000 3211 1234 4 would be a valid IBAN. 
