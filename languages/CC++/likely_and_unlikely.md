# `likely()` and `unlikely()`

In Linux kernel code, one often find calls to likely() and unlikely(), in conditions, like :
`
bvl = bvec_alloc(gfp_mask, nr_iovecs, &idx);
if (unlikely(!bvl)) {
  mempool_free(bio, bio_pool);
  bio = NULL;
  goto out;
}
`
In fact, these functions are hints for the compiler that allows it to correctly optimize the branch, by knowing which is the likeliest one. The definitions of these macros, found in include/linux/compiler.h are the following :
```
#define likely(x)       __builtin_expect(!!(x), 1)
#define unlikely(x)     __builtin_expect(!!(x), 0)
```
The GCC documentation explains the role of `__builtin_expect()` :

 -- Built-in Function: long `__builtin_expect` (long EXP, long C)
 You may use `__builtin_expect` to provide the compiler with branch
 prediction information.  In general, you should prefer to use
 actual profile feedback for this (`-fprofile-arcs`), as
 programmers are notoriously bad at predicting how their programs
 actually perform.  However, there are applications in which this
 data is hard to collect.

 The return value is the value of EXP, which should be an integral
 expression.  The value of C must be a compile-time constant.  The
 semantics of the built-in are that it is expected that EXP == C.
 For example:
     ```
          if (__builtin_expect (x, 0))
            foo ();
    ```
 would indicate that we do not expect to call `foo`, since we
 expect `x` to be zero.  Since you are limited to integral
 expressions for EXP, you should use constructions such as
     ```
          if (__builtin_expect (ptr != NULL, 1))
            error ();
    ```
 when testing pointer or floating-point values.

## How does it optimize things ?

It optimizes things by ordering the generated assembly code correctly, to optimize the usage of the processor pipeline. To do so, they arrange the code so that the likeliest branch is executed without performing any jmp instruction (which has the bad effect of flushing the processor pipeline).

To see how it works, let's compile the following simple C user space program with gcc -O2 :
```
#define likely(x)    __builtin_expect(!!(x), 1)
#define unlikely(x)  __builtin_expect(!!(x), 0)

int main(char *argv[], int argc)
{
   int a;

   /* Get the value from somewhere GCC can't optimize */
   a = atoi (argv[1]);

   if (unlikely (a == 2))
      a++;
   else
      a--;

   printf ("%d\n", a);

   return 0;
}
```
Now, disassemble the resulting binary using objdump -S (comments added by me) :

```
080483b0 <main>:
 // Prologue
 80483b0:       55                      push   %ebp
 80483b1:       89 e5                   mov    %esp,%ebp
 80483b3:       50                      push   %eax
 80483b4:       50                      push   %eax
 80483b5:       83 e4 f0                and    $0xfffffff0,%esp
 //             Call atoi()
 80483b8:       8b 45 08                mov    0x8(%ebp),%eax
 80483bb:       83 ec 1c                sub    $0x1c,%esp
 80483be:       8b 48 04                mov    0x4(%eax),%ecx
 80483c1:       51                      push   %ecx
 80483c2:       e8 1d ff ff ff          call   80482e4 <atoi@plt>
 80483c7:       83 c4 10                add    $0x10,%esp
 //             Test the value
 80483ca:       83 f8 02                cmp    $0x2,%eax
 //             --------------------------------------------------------
 //             If 'a' equal to 2 (which is unlikely), then jump,
 //             otherwise continue directly, without jump, so that it
 //             doesn't flush the pipeline.
 //             --------------------------------------------------------
 80483cd:       74 12                   je     80483e1 <main+0x31>
 80483cf:       48                      dec    %eax
 //             Call printf
 80483d0:       52                      push   %edx
 80483d1:       52                      push   %edx
 80483d2:       50                      push   %eax
 80483d3:       68 c8 84 04 08          push   $0x80484c8
 80483d8:       e8 f7 fe ff ff          call   80482d4 <printf@plt>
 //             Return 0 and go out.
 80483dd:       31 c0                   xor    %eax,%eax
 80483df:       c9                      leave
 80483e0:       c3                      ret
```
Now, in the previous program, replace the unlikely() by a likely(), recompile it, and disassemble it again (again, comments added by me) :
```
080483b0 <main>:
 //             Prologue
 80483b0:       55                      push   %ebp
 80483b1:       89 e5                   mov    %esp,%ebp
 80483b3:       50                      push   %eax
 80483b4:       50                      push   %eax
 80483b5:       83 e4 f0                and    $0xfffffff0,%esp
 //             Call atoi()
 80483b8:       8b 45 08                mov    0x8(%ebp),%eax
 80483bb:       83 ec 1c                sub    $0x1c,%esp
 80483be:       8b 48 04                mov    0x4(%eax),%ecx
 80483c1:       51                      push   %ecx
 80483c2:       e8 1d ff ff ff          call   80482e4 <atoi@plt>
 80483c7:       83 c4 10                add    $0x10,%esp
 //             --------------------------------------------------
 //             If 'a' equal 2 (which is likely), we will continue
 //             without branching, so without flusing the pipeline. The
 //             jump only occurs when a != 2, which is unlikely.
 //             ---------------------------------------------------
 80483ca:       83 f8 02                cmp    $0x2,%eax
 80483cd:       75 13                   jne    80483e2 <main+0x32>
 //             Here the a++ incrementation has been optimized by gcc
 80483cf:       b0 03                   mov    $0x3,%al
 //             Call printf()
 80483d1:       52                      push   %edx
 80483d2:       52                      push   %edx
 80483d3:       50                      push   %eax
 80483d4:       68 c8 84 04 08          push   $0x80484c8
 80483d9:       e8 f6 fe ff ff          call   80482d4 <printf@plt>
 //             Return 0 and go out.
 80483de:       31 c0                   xor    %eax,%eax
 80483e0:       c9                      leave
 80483e1:       c3                      ret
 ```
## How should I use it ?

You should use it only in cases when the likeliest branch is very very very likely, or when the unlikeliest branch is very very very unlikely.
