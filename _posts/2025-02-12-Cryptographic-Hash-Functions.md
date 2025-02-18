---
title: Cryptographic Hash Functions
categories: [network security, cryptography]
tags: [cryptography]     # TAG names should always be lowercase
math: true
---

# Hash-Funktion
Eine **Hash-Funktion** $h:\{0,1\}^* \rightarrow \{0,1\}^m$ bildet einen Bit-String von beliebiger länge auf einen Bit-String der Länge $m$ ab. 

Zum Beispiel
* 010 $\rightarrow 0010111001010011_2$
* $1044_{10} \rightarrow 20_{10}$
* 'geheim' $\rightarrow e8636ea013e682faf61f56ce1cb1ab5c_{16}$

Weil die Anzahl der möglichen Inputstrings viel grösser ist als die Anzahl der möglichen Hashwerte, muss es **Kollisionen** geben, also zwei verschiedene Inputstrings, welche denselben Hashwert erzeugen.   

# Kryptographische Hash-Funktion
Im Gegensatz zu einer Hash-Funkion wie sie in Hashtabellen verwendet wird, z.B. $h(x) = x \; mod\; 1024$ soll eine **kryptographische Hash-Funktion** folgende Anforderungen erfüllen:

1. Einweg-Eigenschaft (preimage resistant)
   - Für einen gegebenen Hashwert $h(x)$ ist es rechnerisch unzumutbar, einen String $y$ zu finden, so dass $h(x)=h(y)$ gilt.
2. schwache Kollisionsresistenz (weak collision resistance)
   - Für einen gegebenen String $x$ ist es rechnerisch unzumutbar einen zweiten String $y$ zu finden, so dass $h(x)=h(y)$ gilt.
3. starke Kollisionsresistenz (strong collision resistance)
   - Es ist unzumutbar, zwei beliebige Strings zu finden, welche denselben Hashwert erzeugen.
4. Pseudozufälligkeit
   - Der erzeugte Hashwert soll sich pseudozufällig verhalten.


# Anwendungen von kryptografischen Hash-Funktionen

* Message Authentication
* Digitale Signaturen
* Passwörter speichern
* Intrusion and virus detection


# Eine simple Hash-Funktion
Die grundlegende Funktionsweise einer kryptographischen Hash-Funktion ist immer dieselbe. Der Inputstring wird in n-bit Blöcke 
aufgeteilt und wenn nötig aufgefüllt. Dann wird für jeden Block iterativ ein interner Hashwert berechnet, basierend auf dem Hashwert des 
vorangehenden Blocks. Sind alle Blöcke mit Hilfe von Permutation und Substitution verarbeitet, wird der n-bit Hashwert zurückgegeben. 

Eine sehr einfache Hash-Funktion ist die bitweise **XOR** Verknüpfung aller Blöcke. Als Resultat erhalten wir einen n-bit Block als 
unseren Hashwert. Wir betrachten die Funktionsweise anhand eines Beispiels mit n = 56 bit und Input 26 x 8 bit = 208 bit.

```
Input: 'Heut ist nicht aller Tage'

als 56-bit Blöcke:  01001000 01100101 01110101 01110100 00100000 01101001 01110011 
                    01110100 00100000 01101110 01101001 01100011 01101000 01110100 
                    00100000 01100001 01101100 01101100 01100101 01110010 00100000 
                    01010100 01100001 01100111 01100101 00001010 
                    --------------------------------------------------------------
Spaltenweise XOR:   01001000 01..................................01110011 00100111
unser 56-bit Hashwert
```
Die Funktion erfüllt keine der Eigenschaften für kryptografische Hash-Funktionen. 

Auch gibt's auf den ersten Blick 
$2^{56}$ verschiedene Hashwerte. Beachte jedoch, dass für Text das MSB meist $0$ ist. D.h es gibt eigentlich nur $2^{56-7}$
verschiedene Werte und die Hash-Funktion nimmt somit weniger Werte an und wird so unsicherer. 


# Brute-Force-Angriffe
Die Resistenz von Hash-Funktionen gegen Brute-Force Angriffe ist durch die Anzahl Hashberechnungen gegeben, welche nötig sind, 
dass die Wahrscheinlichkeit eines Erfolges über 50% liegt. Für m-bit-Hash-Werte ist das wie folgt:
  - preimage resistance: $2^{m-1}$
  - weak collision resistance: $2^{m-1}$
  - strong collision resistance: $\sqrt{2^m}$

## Analyse preimage resistance



## Geburtstagsparadox




