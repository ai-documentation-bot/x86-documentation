# x86 SSE2 extension documentation

## Introduction
SSE2 is a SIMD instruction set extension to the x86 architecture, designed by Intel and introduced in 2001 in their Pentium 4 series of processors shortly after the appearance of AMD's 3DNow!. SSE2 contains 144 new instructions, most of which work on integer and floating point data. SIMD instructions can greatly increase performance when exactly the same operations are to be performed on multiple data objects. Typical applications are digital signal processing and graphics processing.

## Historical background
SSE2 was introduced in 2001 with the Pentium 4 processor (codenamed Willamette). Intel later marketed the instruction set as SSE2 with the Pentium 4's successor, the Pentium 4 with SSE2. 5 more iterations have been released since, each bringing a different set of new instructions. A new iteration has been released with every new processor microarchitecture since.

## Additions to SSE
SSE2 adds 8 new registers to SSE and extends the width of the XMM registers from 128 bits to 256 bits. The new registers are named YMM0 through YMM7. The registers YMM0 through YMM7 are aliased to the registers XMM0 through XMM7 in the SSE instruction set, and conversely the registers XMM0 through XMM7 can be used to hold floating point values for use in SSE2 instructions. The registers YMM0 through YMM7 are also aliased to the registers ZMM0 through ZMM7 in the AVX-512 instruction set, and conversely the registers ZMM0 through ZMM7 can be used to hold floating point values for use in SSE2 instructions.

## How it is used
SSE2 instructions are used to perform operations on multiple data items simultaneously. For example, the following code adds two arrays of 64 bit integers together:

```c
int64_t a[2], b[2], c[2];
int i;
for (i = 0; i < 2; i++)
    c[i] = a[i] + b[i];
```

Using sse2 instructions, the same operation can be performed with the following code:

```c
int64_t a[2], b[2], c[2];
__m128i *pa = (__m128i *)a, *pb = (__m128i *)b, *pc = (__m128i *)c;
*pc = _mm_add_epi64(*pa, *pb);
```

The _mm_add_epi64() function adds the two 64-bit integers in the XMM registers pointed to by pa and pb, and stores the result in the XMM register pointed to by pc. The _mm_add_epi64() function is an intrinsic function that is translated by the compiler into the paddq instruction.

## SSE2 Instructions
SSE2 adds 144 new instructions to SSE. The new instructions are classified into the following categories:
* 
* Data movement instructions
* Arithmetic instructions
* Comparison instructions
* Conversion instructions
* Logical instructions
* Shuffle and unpack instructions
* Cacheability control instructions
* Other instructions

## Examples
### C
The following example shows how to use the _mm_add_epi64() function to add two arrays of 64 bit integers together:

```c
#include <stdio.h>
#include <stdint.h>
#include <emmintrin.h>

int main()
{
    int64_t a[2], b[2], c[2];
    __m128i *pa = (__m128i *)a, *pb = (__m128i *)b, *pc = (__m128i *)c;
    *pc = _mm_add_epi64(*pa, *pb);
    printf("%lld %lld\n", c[0], c[1]);
    return 0;
}
```

### NASM
The following example shows how to use the "paddq" instruction to add two arrays of 64 bit integers together:

```nasm
section .data
    a: dq 1, 2
    b: dq 3, 4
    c: dq 0, 0

section .text
    global _start
_start:
    movdqa xmm0, [a]
    movdqa xmm1, [b]
    paddq xmm0, xmm1
    movdqa [c], xmm0
    mov rdi, c
    mov rsi, format
    xor rax, rax
    call printf
    mov rax, 60
    mov rdi, 0
    syscall

section .data
    format: db "%lld %lld", 10, 0
```

### D
The following example shows how to use the _mm_add_epi64() function to add two arrays of 64 bit integers together:

```d
import std.stdio;
import core.simd;

void main()
{
    int64_t[2] a = [1, 2], b = [3, 4], c = [0, 0];
    __m128i *pa = cast(__m128i *)a.ptr, *pb = cast(__m128i *)b.ptr, *pc = cast(__m128i *)c.ptr;
    *pc = _mm_add_epi64(*pa, *pb);
    writeln(c);
}
```

### Go
The following example shows how to use the _mm_add_epi64() function to add two arrays of 64 bit integers together:

```go
package main

import (
    "fmt"
    "unsafe"
)

func main() {
    a := [2]int64{1, 2}
    b := [2]int64{3, 4}
    c := [2]int64{0, 0}
    pa := (*[2]__m128i)(unsafe.Pointer(&a))
    pb := (*[2]__m128i)(unsafe.Pointer(&b))
    pc := (*[2]__m128i)(unsafe.Pointer(&c))
    *pc = _mm_add_epi64(*pa, *pb)
    fmt.Println(c)
}
```

### Rust
The following example shows how to use the _mm_add_epi64() function to add two arrays of 64 bit integers together:

```rust
use std::mem::transmute;

fn main() {
    let a: [i64; 2] = [1, 2];
    let b: [i64; 2] = [3, 4];
    let c: [i64; 2] = [0, 0];
    let pa: &[__m128i; 2] = unsafe { transmute(&a) };
    let pb: &[__m128i; 2] = unsafe { transmute(&b) };
    let pc: &mut [__m128i; 2] = unsafe { transmute(&c) };
    *pc = _mm_add_epi64(*pa, *pb);
    println!("{:?}", c);
}
```
