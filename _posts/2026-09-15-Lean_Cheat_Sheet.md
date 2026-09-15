---
title: Lean Cheat Sheet
categories: [Lean]
tags: [bsc, unibe]     # TAG names should always be lowercase
math: true
---

# Lean 4 Keyboard & Unicode Cheat Sheet

A practical reference for the Lean 4 symbols and notation you'll use most often.

---

## 🧠 The 15 symbols to learn first

| Meaning                     |               Unicode | Type this in Lean |
| --------------------------- | --------------------: | ----------------- |
| Implication / function type |             $A \to B$ | `->`              |
| If and only if              | $A \leftrightarrow B$ | `<->`             |
| For all                     |           $\forall x$ | `\forall`         |
| There exists                |           $\exists x$ | `\exists`         |
| Not                         |              $\neg P$ | `\not`            |
| And                         |           $P \land Q$ | `\and`            |
| Or                          |            $P \lor Q$ | `\or`             |
| Less than or equal          |             $x \le y$ | `<=`              |
| Greater than or equal       |             $x \ge y$ | `>=`              |
| Not equal                   |             $x \ne y$ | `!=`              |
| Is an element of            |             $x \in S$ | `\in`             |
| Maps to                     |       $x \mapsto x+1$ | `\mapsto`         |
| Definition                  |          $f := \dots$ | `:=`              |
| Lambda body                 |       $x \mapsto x+1$ | `=>`              |
| Left arrow                  |      $x \leftarrow y$ | `<-`              |

> **Tip:** In VS Code, type the backslash command and choose the Unicode symbol from autocomplete.
> For example, type `\forall` and Lean can turn it into $,\forall,$.

---

# 1. Logic & Propositions

These are the symbols you'll encounter constantly in theorem proving.

| Symbol            | Meaning                  | Keyboard   |
| ----------------- | ------------------------ | ---------- |
| $\to$             | implies / function type  | `->`       |
| $\leftrightarrow$ | iff                      | `<->`      |
| $\neg$            | not                      | `\not`     |
| $\land$           | and                      | `\and`     |
| $\lor$            | or                       | `\or`      |
| $\forall$         | for all                  | `\forall`  |
| $\exists$         | there exists             | `\exists`  |
| $\exists!$        | there exists exactly one | `\exists!` |
| $\vdash$          | proves / turnstile       | `\|-`      |
| $\top$            | true                     | `True`     |
| $\bot$            | false                    | `False`    |

### Example

```lean
example : P -> Q := by
  intro h
  -- h : P
  -- goal: Q
```

Mathematically:

$$
P \to Q
$$

means:

> If $P$ is true, then $Q$ is true.

---

# 2. Equality & Comparison

| Symbol | Meaning               | Keyboard |
| ------ | --------------------- | -------- |
| $=$    | equal                 | `=`      |
| $\ne$  | not equal             | `!=`     |
| $<$    | less than             | `<`      |
| $>$    | greater than          | `>`      |
| $\le$  | less than or equal    | `<=`     |
| $\ge$  | greater than or equal | `>=`     |

### Example

```lean
example (x y : Nat) : x <= y -> x < y + 1 := by
  intro h
  omega
```

The proposition is:

$$
x \le y \to x < y+1
$$

---

# 3. Functions & Lambdas

Functions are fundamental in Lean because **proofs are also functions**.

| Notation        | Meaning                     | Keyboard  |
| --------------- | --------------------------- | --------- |
| $A \to B$       | function from $A$ to $B$    | `->`      |
| $x \mapsto x+1$ | maps $x$ to $x+1$           | `\mapsto` |
| `fun x => ...`  | anonymous function / lambda | `=>`      |
| `:=`            | define something            | `:=`      |

### Lambda

You can write:

```lean
fun x => x + 1
```

You may also see:

```lean
fun x ↦ x + 1
```

These express the same basic idea.

**Recommendation:** use `=>` when typing. It's much easier on a normal keyboard.

### Example

```lean
#eval (fun x => x + 1) 5
```

Result:

```text
6
```

---

# 4. Equality Proofs

Equality is written simply with `=`.

```lean
example (x : Nat) : x = x := by
  rfl
```

Mathematically:

$$
x=x
$$

Some useful equality-related notation:

| Lean             | Meaning                             |
| ---------------- | ----------------------------------- |
| `h : x = y`      | $h$ is a proof that $x=y$           |
| `Eq.symm h`      | reverses $x=y$ to $y=x$             |
| `Eq.trans h₁ h₂` | combines $x=y$ and $y=z$ into $x=z$ |
| `rfl`            | proves reflexive equality           |

For example:

```lean
example : y = x -> y = z -> x = z :=
  fun h1 h2 => Eq.trans (Eq.symm h1) h2
```

The proof is:

$$
y=x
\quad\Rightarrow\quad
x=y
$$

then:

$$
x=y,\quad y=z
\quad\Rightarrow\quad
x=z
$$

---

# 5. Sets & Membership

These become increasingly useful as you study mathematics in Lean.

| Symbol      | Meaning                       | Keyboard    |
| ----------- | ----------------------------- | ----------- |
| $\in$       | belongs to / is an element of | `\in`       |
| $\notin$    | does not belong to            | `\notin`    |
| $\subseteq$ | subset                        | `\subseteq` |
| $\subset$   | proper subset                 | `\subset`   |
| $\cup$      | union                         | `\cup`      |
| $\cap$      | intersection                  | `\cap`      |
| $\emptyset$ | empty set                     | `\empty`    |

### Example

```lean
example (x : Nat) (h : x ∈ s) : x ∈ s := h
```

The mathematical statement is:

$$
x \in S \to x \in S
$$

---

# 6. Common Mathematical Symbols

| Symbol       | Meaning                            | Keyboard |
| ------------ | ---------------------------------- | -------- |
| $\mathbb{N}$ | natural numbers                    | `\nat`   |
| $\mathbb{Z}$ | integers                           | `\int`   |
| $\mathbb{Q}$ | rational numbers                   | `\rat`   |
| $\mathbb{R}$ | real numbers                       | `\real`  |
| $\infty$     | infinity                           | `\infty` |
| $\times$     | multiplication / Cartesian product | `\times` |
| $\circ$      | composition                        | `\circ`  |
| $\mid$       | divides                            | `\mid`   |

However, in Lean you'll often use the type names directly:

```lean
Nat
Int
Rat
Real
```

rather than the mathematical Unicode notation.

---

# 7. Arrows

You'll see several kinds of arrows in Lean.

| Symbol            | Meaning                | Keyboard      |
| ----------------- | ---------------------- | ------------- |
| $\to$             | function / implication | `->`          |
| $\leftarrow$      | left arrow             | `<-`          |
| $\leftrightarrow$ | iff                    | `<->`         |
| $\mapsto$         | maps to                | `\mapsto`     |
| $\Rightarrow$     | implies / produces     | `\Rightarrow` |
| $\Leftarrow$      | reverse implication    | `\Leftarrow`  |

### Important distinction

In ordinary Lean code, you'll most frequently use:

```lean
A -> B
```

for:

$$
A \to B
$$

And:

```lean
fun x => x + 1
```

for a function.

---

# 8. `do` Notation

You'll sometimes see:

```lean
let x ← someAction
```

The arrow is:

$$
\leftarrow
$$

For example:

```lean
def main : IO Unit := do
  let stdin ← IO.getStdin
  let stdout ← IO.getStdout
  ...
```

Here `<-` is the keyboard-friendly form of the left arrow.

---

# 9. Logical Connectives — Quick Reference

A useful way to remember the major logical symbols:

| Lean       | Mathematical          | Read it as          |
| ---------- | --------------------- | ------------------- |
| `P -> Q`   | $P \to Q$             | $P$ implies $Q$     |
| `P <-> Q`  | $P \leftrightarrow Q$ | $P$ iff $Q$         |
| `P ∧ Q`    | $P \land Q$           | $P$ and $Q$         |
| `P ∨ Q`    | $P \lor Q$            | $P$ or $Q$          |
| `¬P`       | $\neg P$              | not $P$             |
| `∀ x, P x` | $\forall x,\ P(x)$    | for every $x$       |
| `∃ x, P x` | $\exists x,\ P(x)$    | there exists an $x$ |

---

# 10. VS Code Backslash Shortcuts

One of the nicest features of the Lean VS Code extension is that many Unicode symbols can be entered using a backslash command.

Type:

```text
\forall
```

and select:

$$
\forall
$$

Some useful shortcuts:

```text
\forall     → ∀
\exists     → ∃
\and        → ∧
\or         → ∨
\not        → ¬
\iff        → ↔
\in         → ∈
\notin      → ∉
\mapsto     → ↦
\le         → ≤
\ge         → ≥
\ne         → ≠
\subseteq   → ⊆
\subset     → ⊂
\cup        → ∪
\cap        → ∩
\empty      → ∅
\infty      → ∞
\circ       → ∘
\times      → ×
```

The exact completion available can depend on the Lean editor setup/version, so if one doesn't autocomplete, `#check` or ordinary ASCII notation is often the simplest fallback.

---

# ⭐ The Most Important Rule

**You don't need to memorize the Unicode symbols.**

Lean accepts lots of ordinary keyboard notation directly:

```lean
->      -- →
<=      -- ≤
>=      -- ≥
!=      -- ≠
=>      -- lambda body
:=      -- definition
<-      -- ←
```

So this:

```lean
example : x <= y -> y != x := by
  ...
```

is perfectly reasonable Lean code.

You can gradually start using:

```lean
example : x ≤ y → y ≠ x := by
  ...
```

as you become comfortable with the Unicode notation.

---

# 🏆 Beginner's "Don't Memorize Everything" List

If you're just starting Lean, focus on these:

```text
->       →
=>       =>
:=       :=
<=       ≤
>=       ≥
!=       ≠
\forall  ∀
\exists  ∃
\and     ∧
\or      ∨
\not     ¬
\in      ∈
```

And remember these four ideas:

$$
\begin{aligned}
P \to Q &\quad\text{means "if }P\text{ then }Q"\\
P \land Q &\quad\text{means "P and Q"}\\
P \lor Q &\quad\text{means "P or Q"}\\
\neg P &\quad\text{means "not P"}
\end{aligned}
$$

Once those become familiar, a lot of Lean theorem statements stop looking like mysterious symbols and start looking like ordinary mathematical sentences.
