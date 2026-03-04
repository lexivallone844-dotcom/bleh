# Memory Register

## Ex: 1

main:
  -   mov r10, 25     //store the value 25 in r10 register
  -   mov r11, 62     //store the value 62 in r11 register
  -  jmp mem1        //jumps to mem1 location
  -  mov rax, 1      //store 1 in rax   
  -  ret             //return out of code. If for some reason the jmp does not work, it will return 1

mem1:
   - sub r11, 40     //subtract 40 from r11
   - mov r12, r11    //copy r11 value to r12
   -  cmp r10, r12    //compare the values in r10 and r12
   -  jmple mem2      //jumps to mem2 location if value is less than or equal
   - mov rax, 2      //store 2 in rax   
   - ret             //return out of code. If for some reason the jmp does not work, it will return 2

mem2:
   - mov rax, 0      //store 0 in rax
  - ret             //return out of code. If everything executes properly, it will return 0



(note: memory register reads from right to left)


## Ex: 2

main:
   - mov rax, 16     //16 moved into rax
  -  push rax        //push value of rax (16) onto stack. RSP is pushed up 8 bytes (64 bits)
   - jmp mem2        //jmp to mem2 memory location

mem1:
 -   mov rax, 0      //move 0 (error free) exit code to rax
  -  ret             //return out of code

mem2:
  -  pop r8          //pop value on the stack (16) into r8. RSP falls 8 bytes
   - cmp rax, r8     //compare rax register value (16) to r8 register value (16)
   - je mem1         //jump if comparison has zero bit set to mem1
  -  mov rax, 1      //1 moved into rax
 -   ret             //return out of code


## Ex: 3

**Step-by-Step Breakdown**

**mov ax, 0x3a3b**

This instruction loads the 16-bit hexadecimal value 0x3a3b into the 
 register.
Because 
 is composed of 
 (High byte) and 
 (Low byte), the value is split:

 becomes 0x3a (the upper 8 bits).

 becomes 0x3b (the lower 8 bits).
 
**mov bl, al**

This copies the value of 
 into 
.
Result: 
 now contains 0x3b.
 
**mov cl, ah**

This copies the value of 
 into 
.
Result: 
 now contains 0x3a. 


# DEMO





