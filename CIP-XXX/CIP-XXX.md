---
CIP: ???
Title: Logical operations
Category: Plutus
Status: Proposed
Authors:
    - Koz Ross <koz@mlabs.city>
    - Sparsa Roychowdhury <sparsa@mlabs.city>
Implementors: []
Discussions:
    - https://github.com/cardano-foundation/CIPs/pull/?
Created: 2024-04-24
License: Apache-2.0
---

## Abstract

We describe the semantics of a set of logical operations for Plutus
`BuiltinByteString`s. Specifically, we provide descriptions for:

- Bitwise logical AND, OR, XOR and complement;
- Reading a bit value at a given index;
- Setting a bit value at a given index; and
- Replicating a byte a given number of times.

As part of this, we also describe the bit ordering within a `BuiltinByteString`,
and provide some laws these operations should obey.

## Motivation: why is this CIP necessary?
Currently, Plutus Core has a `BuiltinByteString` type, which is a packed array of bytes, similar to Haskell's ByteString.
We want to implement bit wise operations on `BuiltinByteString` type. In otherwords, we want to achieve bit level granularity on the operations on `BuiltinByteString`.
Some of the operations that we are interested in are bit level OR, AND, NOT, or XOR. There are many added advantage of having bit level granularity which we will discuss in more detail later. All the oprations available on the `BuiltinByteString` works on the byte level granularity.
To explain why we need this bitlevel granularity on the `BuiltinByteString` we use the following example of integer sets.
* Integer sets, also known as bitmap, bitset, or bitvector is used to represent a set of natural numbers. Integer sets can be represented as a `BuiltinByteString` of length N, where N is the maximum number that can be in that set. We can represent if an element `k \leq N` by setting the `k`th bit is set in that `BuiltinByteString`.   There are many advantages when we represet a set using Integer set. 
1. We can do set intersection using bit wise AND operation,
2. We can do set union by using bit wise OR operation,
3. We can do complement using bit wise NOT (assuming finite universe),
4. We can also do set difference using XOR operation (when the number of elements of the sets are same). 
5. Checking membership is as easy as checking an index of that `BuiltinByteString`.
6. Adding and removing an element is as easy as setting and unsetting an index of that `BuiltinByteString`.

TODO: Another 1-2 example uses

## Specification

We describe the proposed operations in several stages. First, we specify a
scheme for indexing individual bits (rather than whole bytes) in a
`BuiltinByteString`. We then specify the semantics of each operation, as well as
giving costing expectations. Lastly, we provide some laws that these operations
are supposed to obey, as well as giving some specific examples of results from
the use of these operations.

### Bit indexing scheme

TODO: Specify this

### Operation semantics

TODO: Specify

### Laws and examples

TODO: Make some

## Rationale: how does this CIP achieve its goals?

TODO: Explain goals relative examples, give descriptions of non-existent
alternatives

## Path to Active

### Acceptance Criteria

TODO: Fill these in (lower priority)

### Implementation Plan

These operations will be implemented by MLabs, to be merged into Plutus Core
after review.

## Copyright

This CIP is licensed under [Apache-2.0](http://www.apache.org/licenses/LICENSE-2.0).
