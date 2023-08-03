# x86 SEE extension documentaion

## Introduction
SSE is a SIMD instruction set extension to the x86 architecture, designed by Intel and introduced in 1999 in their Pentium III series of processors shortly after the appearance of AMD's 3DNow!. SSE contains 70 new instructions, most of which work on single precision floating point data. SIMD instructions can greatly increase performance when exactly the same operations are to be performed on multiple data objects. Typical applications are digital signal processing and graphics processing.

## Historical background
SSE was originally called Katmai New Instructions (KNI), Katmai being the name given to the first Pentium III core revision. Intel later renamed it to Streaming SIMD Extensions. The name was chosen due to the similarity to MMX, Intel's previous SIMD instruction set, which is short for MultiMedia Extensions. SSE was subsequently expanded as Streaming SIMD Extensions, with the second iteration introduced in 2001 with the Pentium 4 processor (codenamed Willamette). Intel later marketed the instruction set as SSE2 with the Pentium 4's successor, the Pentium 4 with SSE2. 5 more iterations have been released since, each bringing a different set of new instructions. A new iteration has been released with every new processor microarchitecture since.

## SSE Registers
SSE adds eight 128-bit registers, known as XMM0 through XMM7, that can be used to hold integers or floating-point numbers. These registers are organized as two sets of four registers, XMM0 through XMM3 are used for floating point operations, while XMM4 through XMM7 are used for integer operations. The registers XMM0 through XMM5 are aliased to the registers ST0 through ST5 in the x87 floating point unit, and conversely the registers ST0 through ST5 can be used to hold floating point values for use in SSE instructions. The registers XMM6 and XMM7 are not aliased to the x87 registers, but are still aliased to the MM6 and MM7 registers in the x87 unit. The registers XMM0 through XMM7 are also aliased to the registers YMM0 through YMM7 in the AVX instruction set, and conversely the registers YMM0 through YMM7 can be used to hold floating point values for use in SSE instructions.

## How it works
SSE instructions operate on data in the XMM registers. The XMM registers are 128 bits wide, and can be used to hold four single-precision floating point numbers, two double-precision floating point numbers, four 32-bit integers, two 64-bit integers, or eight 16-bit integers. The XMM registers are numbered from 0 to 7, and are referred to as XMM0 through XMM7. The XMM registers are organized as two sets of four registers, XMM0 through XMM3 are used for floating point operations, while XMM4 through XMM7 are used for integer operations. The registers XMM0 through XMM5 are aliased to the registers ST0 through ST5 in the x87 floating point unit, and conversely the registers ST0 through ST5 can be used to hold floating point values for use in SSE instructions. The registers XMM6 and XMM7 are not aliased to the x87 registers, but are still aliased to the MM6 and MM7 registers in the x87 unit. The registers XMM0 through XMM7 are also aliased to the registers YMM0 through YMM7 in the AVX instruction set, and conversely the registers YMM0 through YMM7 can be used to hold floating point values for use in SSE instructions.

## How it is used
SSE instructions are used to perform operations on multiple data items simultaneously. For example, the following code adds two arrays of four single-precision floating point numbers together:

```c
float a[4], b[4], c[4];
int i;
for (i = 0; i < 4; i++)
    c[i] = a[i] + b[i];
```

Using sse instructions, the same operation can be performed with the following code:

```c
float a[4], b[4], c[4];
__m128 *pa = (__m128 *)a, *pb = (__m128 *)b, *pc = (__m128 *)c;
*pc = _mm_add_ps(*pa, *pb);
```

The _mm_add_ps() function adds the four single-precision floating point numbers in the XMM registers pointed to by pa and pb, and stores the result in the XMM register pointed to by pc. The _mm_add_ps() function is an intrinsic function that is translated by the compiler into the addps instruction.

## SSE Instructions
SSE instructions are classified into the following categories:
* Data movement instructions
* Arithmetic instructions
* Comparison instructions
* Conversion instructions
* Logical instructions
* Shuffle and unpack instructions
* Cacheability control instructions
* Other instructions

### Data movement instructions
Data movement instructions are used to move data between the XMM registers and memory.

### Arithmetic instructions
Arithmetic instructions are used to perform arithmetic operations on data in the XMM registers.

### Comparison instructions
Comparison instructions are used to compare data in the XMM registers.

### Conversion instructions
Conversion instructions are used to convert data between integer and floating point formats.

### Logical instructions
Logical instructions are used to perform logical operations on data in the XMM registers.

### Shuffle and unpack instructions
Shuffle and unpack instructions are used to rearrange data in the XMM registers.

### Cacheability control instructions
Cacheability control instructions are used to control the caching of data in the XMM registers.

### Other instructions
Other instructions are used to perform miscellaneous operations on data in the XMM registers.

## Examples
### C
```c
#include <xmmintrin.h>

void add(float *a, float *b, float *c)
{
    __m128 *pa = (__m128 *)a, *pb = (__m128 *)b, *pc = (__m128 *)c;
    *pc = _mm_add_ps(*pa, *pb);
}
```

### C++
```c++
#include <xmmintrin.h>

void add(float *a, float *b, float *c)
{
    __m128 *pa = (__m128 *)a, *pb = (__m128 *)b, *pc = (__m128 *)c;
    *pc = _mm_add_ps(*pa, *pb);
}
```

### C#
```c#
using System.Runtime.Intrinsics;

void add(float *a, float *b, float *c)
{
    Vector128<float> va = Vector128.Create(a);
    Vector128<float> vb = Vector128.Create(b);
    Vector128<float> vc = Sse.Add(va, vb);
    vc.CopyTo(c);
}
```

### D
```d
import core.simd;

void add(float *a, float *b, float *c)
{
    simd(float, 4) va = simd(float, 4).load(a);
    simd(float, 4) vb = simd(float, 4).load(b);
    simd(float, 4) vc = va + vb;
    vc.store(c);
}
```

### Go
```go
package main

import (
    "fmt"
    "unsafe"

    "github.com/go-asm/asm"
)

func main() {
    a := [4]float32{1, 2, 3, 4}
    b := [4]float32{5, 6, 7, 8}
    c := [4]float32{}

    asm.MOVUPS(unsafe.Pointer(&a), asm.XMM0)
    asm.MOVUPS(unsafe.Pointer(&b), asm.XMM1)
    asm.ADDPS(asm.XMM1, asm.XMM0)
    asm.MOVUPS(asm.XMM0, unsafe.Pointer(&c))

    fmt.Println(c)
}
```

### NASM
```nasm
section .data
    a: dd 1.0, 2.0, 3.0, 4.0
    b: dd 5.0, 6.0, 7.0, 8.0
    c: dd 0.0, 0.0, 0.0, 0.0

section .text
    global main
    extern printf
    
main:
    movups xmm0, [a]
    movups xmm1, [b]
    addps xmm0, xmm1
    movups [c], xmm0

    mov eax, 0
    ret
```


## Predecessors
Before SSE existed, there was MMX and AMD 3DNow!.

## Successors
SSE was succeeded by SSE2, SSE3, SSE4, AVX, AVX2, AVX-512, FMA, XOP and F16C.

## See also
* [MMX](./mmx.md)
* [SSE2](./sse2.md)
* [SSE3](./sse3.md)
* [SSE4](./sse4.md)
* [AVX](./avx.md)
* [AVX2](./avx2.md)
* [AVX-512](./avx512.md)
* [FMA](./fma.md)
* [XOP](./xop.md)
* [F16C](./f16c.md)
* [AMD 3DNow!](./3dnow.md)
