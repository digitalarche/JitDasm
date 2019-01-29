﻿
Disassembles one or more .NET methods / types to stdout or file(s). It can also create diffable disassembly.

Tips:

- .NET Core: Disable tiered compilation in target process: COMPlus_TieredCompilation=0
- Use release builds
- Target process must be a .NET Framework 4.5+ / .NET Core process

```cmd
jitdasm -p 1234 --diffable -m ConsoleApp1 --method TestMethod
```

```asm
; ================================================================================
; ConsoleApp1.Program.TestMethod(System.String[])
; 87 (0x57) bytes

        push      rdi
        push      rsi
        push      rbx
        sub       rsp,20h
        mov       rsi,rcx
        mov       rcx,offset <diffable-addr>
        mov       edx,58h
        call      CORINFO_HELP_CLASSINIT_SHARED_DYNAMICCLASS
        mov       rcx,offset <diffable-addr>
        mov       rcx,[rcx]
        mov       ecx,[rcx+8]
        call      System.Console.WriteLine(Int32)
        xor       edi,edi
        mov       ebx,[rsi+8]
        test      ebx,ebx
        jle       LBL_1

LBL_0:
        movsxd    rcx,edi
        mov       rcx,[rsi+rcx*8+10h]
        call      System.Console.WriteLine(System.String)
        inc       edi
        cmp       ebx,edi
        jg        LBL_0

LBL_1:
        add       rsp,20h
        pop       rbx
        pop       rsi
        pop       rdi
        ret
```

Known issues:

- Generic methods and methods in generic types can't be disassembled. It's possibly a DAC API limitation. Try `--heap-search` to find instantiated generic types on the heap.