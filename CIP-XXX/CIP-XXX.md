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
Some of the operations that we are interested in are, bit level OR, AND, NOT, or XOR. There are many added advantage of having bit level granularity which we will discuss in more detail later. Unfortunately, all oprations available on the `BuiltinByteString` works on the byte level granularity. So we need this CIP to bridge the gap.
To explain the usefulness of this granulaity to manipulate `BuiltinByteString`, we use the following example of integer sets.
#### Integer sets
Integer sets, also known as bitmap, bitset, or bitvector is used to represent a set of natural numbers. Integer sets can be represented as a `BuiltinByteString` of length $N$, where $N\in \mathbb{N}$ is the maximum possible number in that set. We can represent membership of an element $k \leq N$ by setting the $k^{th}$ bit is set in that `BuiltinByteString`.  For example, assume $ x = \{1,3,4,5\}$ be an integer set. We can represent this set using a byte string of size $8$ as $01011100$. Note that the $1^{st}$, $3^{rd}$, $4^{th}$ and $5^{th}$ bits are set and rest are zero. There are many advantages when we represet a set using Integer set. 
1. Representing the empty set and the universe. We can use the `0x00`  or $00000000$ to represent an empty set, and `0xFF` or $11111111$ as the universe that can be represetend using $8$ bit byte string. 
2. We can do set intersection using bit wise AND operation. Assume two sets $x = \{2,5,6\}$ and $y = \{2,4,5\}$, then $x \cap y = \{2,5\}$. The following example shows how we can represent set union using the bit wise AND operation.
    ```mermaid 
    graph LR 
    A[00100110] --> C[Bitwise AND]
    B[00101100] --> C
    C --> D[00100100]
    
    ```
3. We can do set union by using bit wise OR operation. Let us use the same sets $x$ and $y$. Then the unition $x \cup y = \{2,4,5,6\}$. The following image shows how the bitwise OR operation can be used to compute this union.
    ```mermaid 
    graph LR 
    A[00100110] --> C[Bitwise OR]
    B[00101100] --> C
    C --> D[00101110]
    
    ```

4. We can do complement using bit wise NOT (assuming finite universe). Let us take the set $x = \{2,5,6\}$, and assume that the universal set be $\{0,1,2,3,4,5,6,7\}$. Then, the complement of the set $x$, is $\neg x = \{0,1,3,4,7\}$. The following figure represents shows how we can represent these sets in byte strings and how bitwise NOT operation can be used to compute the complement of a set.
    ```mermaid 
    graph LR 
    A[00100110] --> C[Bitwise NOT]
    C --> D[11011001]
    
    ```

5. We can also do set difference using XOR operation (when we are doing a symmetric difference). Just like previous examples we will take the sets $x$ and $y$. Then the symmetric difference between them would be $\{4,6\}$. As we can see in the next figure, the symmetric difference can be implemented using XOR bit wise operator.
     ```mermaid 
    graph LR 
    A[00100110] --> C[Bitwise XOR]
    B[00101100] --> C
    C --> D[00001010]
    ```
6. Checking membership is as easy as checking an index of that `BuiltinByteString`. As per our definition, if we have $k$ in the set, then we are setting $k^{th}$ element in the byte string to represent the membership of $k$ in the integer set. So checking the membership for any element $k$ boils down to checking two things, (a) the size of the byte string and (b) the bit at the position $k$. 
6. Adding and removing an element is as easy as setting and unsetting an index of that `BuiltinByteString`. For example if we want to add the element $1$ in the set $x$ then we only have to set the $1^{st}$ element of the bytestring representing the set $x$. On the other hand if we want to remove the emelent $2$ from the set $x$ then we will unset the $2^{nd}$ element from the set $x$.

Clearly this is very efficient in terms of both space and time. 
#### Bit packing
Sometimes you have small integer values (often 0-7) that you want to store efficiently. By packing multiple values (2-4 bits each) into a single byte, you can save space. Bitwise operations like shifting and masking allow you to extract and manipulate these individual values within the byte string. 
#### Other examples


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
