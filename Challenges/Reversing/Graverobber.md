# Graverobber
radare2
- i: Displays general metadata of the binary.
- ie: Shows the program’s entrypoints.
- im: Identifies the address of the main function.
- iz: Lists all strings found in the binary.
- aaa: Performs a full analysis of the binary.
- axt: Shows cross-references to a given element (e.g., where a string is used).
- pdf @ main: Disassembles and displays the main function.
- xc @ main: Displays the call graph associated with the main function.
```bash
sebastianrojas@sebas:~/Descargas/challenge/rev_graverobber$ radare2 robber 
WARN: Relocs has not been applied. Please use `-e bin.relocs.apply=true` or `-e bin.cache=true` next time
[0x00001060]> aaa
INFO: Analyze all flags starting with sym. and entry0 (aa)
INFO: Analyze imports (af@@@i)
INFO: Analyze entrypoint (af@ entry0)
INFO: Analyze symbols (af@@@s)
INFO: Analyze all functions arguments/locals (afva@@@F)
INFO: Analyze function calls (aac)
INFO: Analyze len bytes of instructions for references (aar)
INFO: Finding and parsing C++ vtables (avrr)
INFO: Analyzing methods (af @@ method.*)
INFO: Recovering local variables (afva@@@F)
INFO: Type matching analysis for all functions (aaft)
INFO: Propagate noreturn information (aanr)
INFO: Use -AA or aaaa to perform additional experimental analysis
[0x00001060]> afl
0x00001030    1      6 sym.imp.puts
0x00001040    1      6 sym.imp.__stack_chk_fail
0x00001050    1      6 sym.imp.stat
0x00001060    1     37 entry0
0x0000126c    1     13 sym._fini
0x00001159    9    275 main
0x00001000    3     27 sym._init
0x00001150    5     60 entry.init0
0x00001100    5     55 entry.fini0
0x00001090    4     34 fcn.00001090
[0x00001060]> pdf @main
            ; ICOD XREF from entry0 @ 0x1078(r)
┌ 275: int main (int argc, char **argv, char **envp);
│ afv: vars(12:sp[0x10..0xec])
│           0x00001159      55             push rbp
│           0x0000115a      4889e5         mov rbp, rsp
│           0x0000115d      4881ecf000..   sub rsp, 0xf0
│           0x00001164      64488b0425..   mov rax, qword fs:[0x28]
│           0x0000116d      488945f8       mov qword [canary], rax
│           0x00001171      31c0           xor eax, eax
│           0x00001173      48c745b000..   mov qword [var_50h], 0
│           0x0000117b      48c745b800..   mov qword [var_48h], 0
│           0x00001183      48c745c000..   mov qword [var_40h], 0
│           0x0000118b      48c745c800..   mov qword [var_38h], 0
│           0x00001193      48c745d000..   mov qword [var_30h], 0
│           0x0000119b      48c745d800..   mov qword [var_28h], 0
│           0x000011a3      48c745e000..   mov qword [var_20h], 0
│           0x000011ab      48c745e800..   mov qword [var_18h], 0
│           0x000011b3      c745f00000..   mov dword [var_10h], 0
│           0x000011ba      c7851cffff..   mov dword [var_e4h], 0
│       ┌─< 0x000011c4      eb71           jmp 0x1237
│       │   ; CODE XREF from main @ 0x1240(x)
│      ┌──> 0x000011c6      8b851cffffff   mov eax, dword [var_e4h]
│      ╎│   0x000011cc      4898           cdqe
│      ╎│   0x000011ce      488d148500..   lea rdx, [rax*4]
│      ╎│   0x000011d6      488d05632e..   lea rax, obj.parts          ; 0x4040 ; U"HTB{br34k1n9_d0wn_th3_sysc4ll5}"
│      ╎│   0x000011dd      8b1402         mov edx, dword [rdx + rax]
│      ╎│   0x000011e0      8b851cffffff   mov eax, dword [var_e4h]
│      ╎│   0x000011e6      01c0           add eax, eax
│      ╎│   0x000011e8      4898           cdqe
│      ╎│   0x000011ea      885405b0       mov byte [rbp + rax - 0x50], dl
│      ╎│   0x000011ee      8b851cffffff   mov eax, dword [var_e4h]
│      ╎│   0x000011f4      01c0           add eax, eax
│      ╎│   0x000011f6      83c001         add eax, 1
│      ╎│   0x000011f9      4898           cdqe
│      ╎│   0x000011fb      c64405b02f     mov byte [rbp + rax - 0x50], 0x2f ; '/'
│      ╎│   0x00001200      488d9520ff..   lea rdx, [var_e0h]
│      ╎│   0x00001207      488d45b0       lea rax, [var_50h]
│      ╎│   0x0000120b      4889d6         mov rsi, rdx
│      ╎│   0x0000120e      4889c7         mov rdi, rax
│      ╎│   0x00001211      e83afeffff     call sym.imp.stat
│      ╎│   0x00001216      85c0           test eax, eax
│     ┌───< 0x00001218      7416           je 0x1230
│     │╎│   0x0000121a      488d05e70d..   lea rax, str.We_took_a_wrong_turning_ ; 0x2008 ; "We took a wrong turning!"
│     │╎│   0x00001221      4889c7         mov rdi, rax                ; const char *s
│     │╎│   0x00001224      e807feffff     call sym.imp.puts           ; int puts(const char *s)
│     │╎│   0x00001229      b801000000     mov eax, 1
│    ┌────< 0x0000122e      eb26           jmp 0x1256
│    ││╎│   ; CODE XREF from main @ 0x1218(x)
│    │└───> 0x00001230      83851cffff..   add dword [var_e4h], 1
│    │ ╎│   ; CODE XREF from main @ 0x11c4(x)
│    │ ╎└─> 0x00001237      8b851cffffff   mov eax, dword [var_e4h]
│    │ ╎    0x0000123d      83f81f         cmp eax, 0x1f
│    │ └──< 0x00001240      7684           jbe 0x11c6
│    │      0x00001242      488d05df0d..   lea rax, str.We_found_the_treasure___I_hope_its_not_cursed_ ; 0x2028 ; "We found the treasure! (I hope it's not cursed)"
│    │      0x00001249      4889c7         mov rdi, rax                ; const char *s
│    │      0x0000124c      e8dffdffff     call sym.imp.puts           ; int puts(const char *s)
│    │      0x00001251      b800000000     mov eax, 0
│    │      ; CODE XREF from main @ 0x122e(x)
│    └────> 0x00001256      488b55f8       mov rdx, qword [canary]
│           0x0000125a      64482b1425..   sub rdx, qword fs:[0x28]
│       ┌─< 0x00001263      7405           je 0x126a
│       │   0x00001265      e8d6fdffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       │   ; CODE XREF from main @ 0x1263(x)
│       └─> 0x0000126a      c9             leave
└           0x0000126b      c3             ret

```
