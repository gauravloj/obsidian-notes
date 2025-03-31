building assembly
- Simple assembly file
```asm
.intel_syntax noprefix
.global _start
_start:
	mov rdi, 42
	mov rax, 60
	syscall
```

- Compiling
```sh
# It will generate the 'asm.o' object file
as -o asm.o asm.s

# It will link necessary object file to create the final executable file
ld -o exe asm.o

# Or it can directly be compiled using gcc
gcc -static -nostdlib -o exe asm.s

```


- Find decimal value for flags used in opening a file
```c
#include ‹stdio.h›
#include ‹fcntl.h›

int main() {
	printf("ORDONLY is: %d\n", O_RDONLY);
}
```

-
