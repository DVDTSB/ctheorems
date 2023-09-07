# typst-theorems
Fork of [https://github.com/sahasatvik/typst-theorems](https://github.com/sahasatvik/typst-theorems) for making it an official typst plugin.


An implementation of numbered theorem environments in
[typst](https://github.com/typst/typst).
Copy and import the [theorems.typ](theorems.typ) file to use in your own projects.

### Features
- Numbered theorem environments can be created and customized.
- Environments can share the same counter, via same `identifier`s.
- Environment counters can be _attached_ (just as subheadings are attached to headings) to other environments, headings, or keep a global count via `base`.
- The depth of a counter can be manually set, via `base_level`.
- Environment numbers can be referenced, via `#thmref(<label>)[]`.
 Currently, the `<label>` must be placed _inside_ the environment.

## Manual and Examples
Get acquainted with `typst-theorems` by checking out the minimal example below!

You can read the [manual](manual.pdf) for a full walkthrough of functionality offered by this module; flick through [manual_examples](manual_examples.pdf) and its [typ file](manual_examples.typ) to just see the examples.

The [differential_calculus.typ](differential_calculus.typ) ([render](differential_calculus.pdf)) project provides a practical use case. _(Hastily translated from my notes written in LaTeX)_

![basic example](basic.png)

### Preamble
```
#import "theorems.typ": *

#set page(width: 16cm, height: auto, margin: 1.5cm)
#set heading(numbering: "1.1.")

#let theorem = thmbox("theorem", "Theorem", fill: rgb("#eeffee"))
#let corollary = thmplain(
  "corollary",
  "Corollary",
  base: "theorem",
  titlefmt: strong
)
#let definition = thmbox("definition", "Definition", inset: (x: 1.2em, top: 1em))

#let example = thmplain("example", "Example").with(numbering: none)
#let proof = thmplain(
  "proof",
  "Proof",
  base: "theorem",
  bodyfmt: body => [#body #h(1fr) $square$]
).with(numbering: none)
```

### Document
```
= Prime numbers

#definition[
  A natural number is called a _prime number_ if it is greater than 1
  and cannot be written as the product of two smaller natural numbers.
]
#example[
  The numbers $2$, $3$, and $17$ are prime.
  #thmref(<cor_largest_prime>)[Corollary] shows that this list is not
  exhaustive!
]

#theorem(name: "Euclid")[
  There are infinitely many primes.
]
#proof[
  Suppose to the contrary that $p_1, p_2, dots, p_n$ is a finite enumeration
  of all primes. Set $P = p_1 p_2 dots p_n$. Since $P + 1$ is not in our list,
  it cannot be prime. Thus, some prime factor $p_j$ divides $P + 1$.  Since
  $p_j$ also divides $P$, it must divide the difference $(P + 1) - P = 1$, a
  contradiction.
]

#corollary[
  There is no largest prime number. <cor_largest_prime>
]
#corollary[
  There are infinitely many composite numbers.
]

```

