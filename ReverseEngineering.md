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

## Static Analysis

- (download file, go to file in cmd line, and analyze it without opening file)

**strings.exe -a -nobanner (file) | findstr /i enter**

**strings.exe -a -n 7 -nobanner (file) | select -first 10**

- (note: -n # : of how long you want word to be)

## Behavioral Analysis

//Run it

- (open Ghidra, create project, import file, double click on exe file, click analyze, )

- (analyze the strings, up on top click **Search**, **Strings**, and then enter what you want to be filtered at the bottom)

- ( when you found the string, in Ghidra it will go straight to it, double-click the function to read the script tied to function)

**in the decompile for the funciton, you can right-click and convert hex -> decimal**

- (note: you need to look for how the script is ran from end to beginning, can edit the var and functions in the script to make it more understandable)

**local_1c = userinput**

**local_8 = userinput**

- (note: look for the functions that stick out)

  
## Patching

- (note: main goal is to alter what the function does, which you will want to creat a success message everytime)

- (Start by working backwards)

- Found success message, navigate to success function, found condition that sets it in the C code, where gonna click it so it highlights in assembly.
- 
## How to change assembly

- Right-click the register or number in the assembly > then click **patch instruction** > change number to match the register (look for something like CMP(which subtracts values and will = 0 if true) 
- Change hex number to EAX
- Go to FIle at top > Format PE > Change the location > save it and run it


  (when cycling through analysis check to see when things are read. Look to see how the files change when you press enter. Values will be pulled from file or from registry)
