...

### Syscalls - `write` and `read`

```nasm
_start:
    mov     rax, 0         ; first arg for syscall, 0 = read
    mov     rdi, 0         ; second arg for syscall, first for read, 0 = stdin
    mov     rsi, in        ; third arg for syscall, second for read, in = .bss resb buffer address, [in] for contents
    mov     rdx, 64        ; fourth arg for syscall, third for read, 64 = size of buffer, max bytes to read
    syscall                ; perform syscall, stored no of bytes read in rax

    mov     rdx, rax       ; mov no of bytes read from rax to rdx, rdx = len of bytes to write
    mov     rax, 1         ; 1 = write() syscall
    mov     rdi, 1         ; 1 = stdout
                           ; did not mov rsi, in cuz rsi already has that address from the previous mov
    mov     rdx, 64        ; len of bytes to write
    syscall

    mov     rax, 60        ; 60 = exit() syscall
    mov     rdi, 0         ; rdi has the exit code in linux systems, we clear rdi to 0 to indicate successful exit
    syscall

section .bss
in: resb 64
```

### Addressing - Functions and Stack

```c
// source
void bar(int a, int b) {
    int x, y;

    x = 555;
    y = a+b;
}

void foo(void) {
    bar(111,222);
}
```

```nasm
; AT&T Syntax (source, destination) / compiles to:
bar:
    pushl   %ebp           ; EBP pushed to stack for backup
    movl    %esp, %ebp     ; ESP content moved to EBP, which is now base address for current frame/point in stack / this function call, sets up EBP as "frame pointer"
    subl    $16, %esp      ; decrement ESP by 16 bytes (or whatever size you need) to reserve space for local variables
    movl    $555, -4(%ebp) ; negative address would be in current stack frame, stack grows top to bottom
    movl    12(%ebp), %eax ; positive offset refers to address of caller, previous stack frame, in this case this is one of the arguments
    movl    8(%ebp), %edx
    addl    %edx, %eax
    movl    %eax, -8(%ebp)
    leave                  ; restores ESP & EBP
    ret                    ; pops return address into EIP

foo:
    pushl   %ebp
    movl    %esp, %ebp
    subl    $8, %esp
    movl    $222, 4(%esp)
    movl    $111, (%esp)
    call    bar
    leave
    ret
```

- %ebp -> base pointer
- %esp -> stack pointer

stack grows top to bottom and so items are pushed at the bottom
