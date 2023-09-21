# Writeup for: TryHackMe 0x41haz

Difficulty: Easy

Cathegory: Reverse-Engeneering

File Attached: 0x41haz.0x41haz






## Step 1 => We start with Unix:

file 0x41haz.0x41haz

Which Gives us: 

0x41haz.0x41haz: ELF 64-bit MSB *unknown arch 0x3e00* (SYSV)


This is an so called "Elf" File. Elf stands for Executable and Linkable Format and is an standart Binary File for Most Unix Systems.

## Step 2 => Execution

chmod +x 0x41haz.0x41haz
./0x41haz.0x41haz

This lets us Execute the File, But it wants a Password

## Step 3: Analysing

The 
>> MSB *unknown arch 0x3e00* << 

Part is the interesting one. To understand it we need to dig a Bit down to Elf Format.

This might help you with it :) 

https://en.wikipedia.org/wiki/Executable_and_Linkable_Format

You can see there, that Elf Files typically start with an Elf-Header.

Parts of The Elf-Header can be changed without Compromising The Entire File. 


After a Short Google of 

MSB *unknown arch 0x3e00* 

We find

https://pentester.blog/?p=247

Which explains us that in the Header:

The 5th byte defines format 32 bits (1) or 64 bits (2)

The 6th byte defines endianness LSB (1)  MSB (1)



Lets open it in hexeditor: 

hexeditor 0x41haz.0x41haz

As you can see in the Hexedit Field, The Sixth Bit is 02 . We saw earlier :

ELF 64-bit MSB *unknown arch 0x3e00* (SYSV)

When We now Think About that MSB is typically (1) there is a (2) we now what to do!


It can only be 01, so lets Change that!

Right After that we use again:

file 0x41haz.0x41haz

but THIS time:


0x41haz.0x41haz: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=6c9f2e85b64d4f12b91136ffb8e4c038f1dc6dcd, for GNU/Linux 3.2.0, stripped

Well this IS some more info






# Way 1: Doing it With Ghidra:

Lets fire up Ghidra and open The Programm in it.

In Normaly Case we look for a Main function first.

Well.. We dont have one but we have an "entry" function.. Lets take a look at it first...

For everybody who is as an amateur as I am:



void entry(undefined8 param_1,undefined8 param_2,undefined8 param_3)

{
  undefined8 in_stack_00000000;
  undefined auStack8 [8];
  
  __libc_start_main(FUN_00101165,in_stack_00000000,&stack0x00000008,&LAB_00101240,&DAT_001012a0,
                    param_3,auStack8);
  do {
                    /* WARNING: Do nothing block with infinite loop */
  } while( true );
}




In Ghidra you can see Most functions being called by:

Wait for Ghidra to finish the analyzing completley!



FUN_XXXXXXXXXXX

here you can in fact see even one :)

FUN_00101165


Lets Focus on this one!

Oh Boy! Its Our Programm!


And 3 Variables are Declared!

  local_1e = 0x6667243532404032;
  local_16 = 0x40265473;
  local_12 = 0x4c;

Hover with your Mouse over them!

2@@25$gf
sT&@
L


Which makes: 

2@@25$gfsT&@L

Try it! 


There also is another way in using Radare2 But I will keep that for another day....



# Way 2: Doing it With Radare2


First we open the file with R2

In case you havent Radare2 installed:

sudo apt install radare2

Open The Program with radare2

r2 -d 0x41haz.0x41haz

Next we list all the Functions of the Program:

afl

Next we print out the Main Function of the Program

pdf @main

Now We look at some Fancy Colors! :)

What We Need are:

local_1e
local_16
local_12

They are clearly visible here and the red Text after them are the 3 Parts of the Flag