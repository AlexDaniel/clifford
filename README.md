Geometric Algebra in Perl 6
===========================

[![Build Status](https://travis-ci.org/grondilu/clifford.svg)](https://travis-ci.org/grondilu/clifford)

This module is an attempt to implement basic [Geometric
Algebra](http://en.wikipedia.org/wiki/Geometric_Algebra) in Perl 6.

With this module you can create euclidean and anti-euclidean vectors of
arbitrary size.  You can then add and substract them, as well as multiplying
them by real scalars as you would do with any vectors, but you can also
multiply and divide as made possible by the geometric algebra.

Euclidean space
---------------

The module exports an array constant `@e` which serves as an orthonormal basis
for an Euclidean space of infinite, countable dimension.

    use Clifford;

    say @e[0]**2;         # +1
    say @e[0] + 2*@e[1];  # e0+2*e1
    say @e[0]*@e[1];      # e0∧e1

Anti-Euclidean space
--------------------

The module also exports an array constant `@ē` which serves as an orthonormal
basis for an anti-Euclidean space of infinite, countable dimension.  This
space is orthogonal to the Euclidean space.

    use Clifford;

    say @ē[0]**2;        # -1
    say @e[0] + @ē[0];   # e0 + ē0
    say @e[0]*@ē[0];     # e0∧ē0

The `ē` character is the voyel `e` with a [macron](https://en.wikipedia.org/wiki/Macron).
It is available as a default digraph on [Vim](http://www.vim.org) as `e-`.

Minkowski plane
---------------

The module exports two constants `no` and `ni` which form a null basis of a
[Minkowski plane](https://en.wikipedia.org/wiki/Minkowski_plane).  This plane
is orthogonal to both the Euclidean space and the anti-Euclidean space.

    use Clifford;
    say no;                 # 𝑜
    say ni;                 # ∞
    say no**2;              # 0
    say ni**2;              # 0
    say no*@e[0];           # 𝑜∧e0
    say ni*@ē[0];           # ∞∧ē0
    say no*ni;              # -1+𝑜∧∞
    say (no*ni + ni*no)/2   # -1

Grade projection
----------------

The `AT-POS` method returns the grade projection:

    say (no + @e[1] + @e[0]*@e[1])[1];   # 𝑜+e1

Operations
----------

The module *does not* define any of the various products derived from the
geometric product.  However, the user can fairly easily define them.

For instance, for the inner product of two vectors:

    sub infix:<·> { ($^a*$^b + $a*$b)/2 }
    say @e[0]·(@e[0] + @e[1]);   # 1

The above example can only work if the arguments are vectors.  Ensuring this is
a bit involved and would probably cost in terms of performance.  That's why the
module does not try to find a compromise and let that decision to the user.

External links
--------

* [Geometric Algebra for Computer science](http://www.geometricalgebra.net) : a
  website and a book that helped for this project ;
* [Versor](https://github.com/wolftype/versor), a C++ implementation of the
  above reference.  This uses advanced optimizations with precompilation and
stuff.  Ideally it should be possible to copy ideas from this, but it will need
work.
