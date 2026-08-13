# LDA
The LDA instruction stands for Load Accumulator. It is one of the most important instructions in MIXAL. Its job is to copy data from a specific memory location into the Accumulator Register (rA). [1] 
Here is the complete guide on how it works and how to use it.
## The Anatomy of LDA
The full syntax for the LDA instruction looks like this:
LDA ADDRESS,I(F)

* ADDRESS: The memory location you want to read.
* ,I (Optional Index Register): The register (1 to 6) used to shift the address.
* (F) (Optional Field Specification): Tells MIX which specific bytes of the memory word to load. If you leave this out, it defaults to (0:5), which means the whole word (the sign and all 5 bytes).

------------------------------
## Common Ways to Use LDA## 1. Full Word Load (The Default)
Loads the entire memory word (Sign + 5 bytes) into rA. [2] 

LDA   ARRAY        Loads the whole word at ARRAY into rA

## 2. Indexed Load (For Arrays)
Adds the value of an index register to the base address before loading.

LDA   ARRAY,1      If rI1 is 3, this loads the word at ARRAY+3

## 3. Partial Word Load (Using F-Fields)
You can load just a few bytes of a word. The rest of the register gets filled with zeros, and the sign behaves according to special rules.

* (0:0): Loads only the sign bit.
* (1:5): Loads bytes 1 through 5 (ignores the original sign, sets rA sign to +).
* (4:5): Loads the last two bytes. They move to the right side of rA, and the left sides become zero.

LDA   ARRAY(1:3)   Loads bytes 1, 2, and 3 into the right side of rA

------------------------------
## Code Demonstration
Here is a short, complete MIXAL program. It sets up three variables with different numbers and uses LDA in three different ways to manipulate the Accumulator.

* --- Define Data Block ---
         ORIG  3000         Start data at memory address 3000
X        CON   -12345       A full negative number word
Y        CON   500          A small positive number
Z        CON   0            A word initialized to zero

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0
         
* Demo 1: Basic Full Load
         LDA   X            rA now holds the exact word at X (-12345)
         STA   Z            Copy rA into Z (Z is now -12345)

* Demo 2: Partial Load (F-Field)
         LDA   X(1:5)       Loads only bytes 1-5 of X (ignores the minus sign)
*                           rA now holds +12345 (positive)

* Demo 3: Address Modification with Indexing
         ENT2  1            Set Index Register 2 (rI2) = 1
         LDA   X,2          Address is X + 1, which points to Y
*                           rA now holds the value of Y (500)

         HLT                Stop the machine
         END                End of the program text

Would you like to see:

* How to use STA (Store Accumulator) to save data back into your array?
* How the F-field shifting works visually inside the rA register bytes?


[1] [https://www.atlas.org](https://www.atlas.org/spaces/solve/8085-microprocessor-instructions-and-programming-dCxgZTPGhBxM6RUDydd9sS)
[2] [https://esolangs.org](https://esolangs.org/wiki/MIX_%28Knuth%29)


# ADD
The ADD instruction in MIXAL adds a value from memory directly to whatever is currently sitting inside the Accumulator Register (rA). The final result is then saved back into rA. [1, 2] 
Here is the complete guide on how it works and how to use it.
## The Anatomy of ADD
The full syntax for the ADD instruction looks like this:
ADD ADDRESS,I(F)

* ADDRESS: The memory location holding the number you want to add.
* ,I (Optional Index Register): The register (1 to 6) used to shift the target address.
* (F) (Optional Field Specification): Tells MIX which specific bytes of the memory word to read before adding. If omitted, it defaults to (0:5), which uses the whole word (sign and all 5 bytes).

## How ADD Behaves

   1. MIX looks at the value in rA.
   2. MIX looks at the value in the specified memory address.
   3. It adds them together algebraically (taking negative and positive signs into account).
   4. The answer replaces the old value in rA.
   5. Overflow Warning: If the answer is too big to fit into a single MIX word, the internal Overflow Toggle is turned on.

------------------------------
## Code Demonstration
Here is a short, complete MIXAL program. It sets up numbers in memory and uses ADD in three different ways: a basic addition, a negative addition (which works like subtraction), and an indexed addition. [3] 

* --- Define Data Block ---
         ORIG  2000         Start data at memory address 2000
NUM1     CON   150          A positive number
NUM2     CON   -50          A negative number
NUM3     CON   300          Another positive number

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0

* Demo 1: Basic Addition
         LDA   NUM1         Load 150 into rA
         ADD   NUM3         Add NUM3 (300) to rA
*                           rA now holds: 150 + 300 = 450

* Demo 2: Adding a Negative Number (Algebraic)
         ADD   NUM2         Add NUM2 (-50) to rA
*                           rA now holds: 450 + (-50) = 400

* Demo 3: Indexed Addition
         ENT4  2            Set Index Register 4 (rI4) = 2
         LDA   NUM1         Reset rA to 150
         ADD   NUM1,4       Address is NUM1 + 2, which points to NUM3
*                           This adds NUM3 (300) to rA
*                           rA now holds: 150 + 300 = 450

         HLT                Stop the machine
         END                End of the program text

------------------------------
Would you like to see:

* How to use SUB (Subtract) and how it handles signs differently?
* How to check if an addition caused an overflow using jump commands?


[1] [https://www.youtube.com](https://www.youtube.com/watch?v=QWd3XfAa88o)
[2] [https://esolangs.org](https://esolangs.org/wiki/MIX_%28Knuth%29)
[3] [https://www.solisplc.com](https://www.solisplc.com/tutorials/plc-programming-computational-mathematical-instructions-add-addition)


# STA
The STA instruction stands for Store Accumulator. It does the exact opposite of LDA. Instead of reading from memory, STA takes the data currently sitting inside the Accumulator Register (rA) and copies it into a specific memory location.
Here is the complete guide on how it works and how to use it.
## The Anatomy of STA
The full syntax for the STA instruction looks like this:
STA ADDRESS,I(F)

* ADDRESS: The destination memory location where you want to save the data.
* ,I (Optional Index Register): The register (1 to 6) used to shift the destination address.
* (F) (Optional Field Specification): Tells MIX which specific bytes of the memory destination should be overwritten. If omitted, it defaults to (0:5), which overwrites the entire word.

## How the F-Field Works with STA (Crucial Rule)
When you use a partial field like (1:3) with STA, it behaves differently than LDA:

   1. It takes the rightmost bytes of the register rA.
   2. It writes them into the specified byte slots of the memory word.
   3. The rest of the bytes in that memory word remain completely unchanged.

------------------------------
## Code Demonstration
Here is a complete MIXAL program. It demonstrates a basic store, an indexed store into an array, and a partial field store that modifies only a piece of a memory word.

* --- Define Data Block ---
         ORIG  1000         Start data at memory address 1000
SAVESPOT CON   0            An empty word to save data into
ARRAY    CON   0            Index 0 of a small array
         CON   0            Index 1 of a small array
TARGET   CON   11111        A word filled with ones (to demo partial store)

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0

* Demo 1: Basic Full Store
         ENTA  555          Load the number 555 into rA
         STA   SAVESPOT     Copy rA into memory at SAVESPOT
*                           SAVESPOT now holds the value 555

* Demo 2: Indexed Store (Saving to an Array)
         ENT3  1            Set Index Register 3 (rI3) = 1
         ENTA  999          Load 999 into rA
         STA   ARRAY,3      Address is ARRAY + 1 (the second slot)
*                           The second slot of ARRAY now holds 999

* Demo 3: Partial Store (F-Field modification)
         ENTA  7            Load 7 into rA (bytes look like: + 0 0 0 0 7)
         STA   TARGET(5:5)  Write the last byte of rA into the last byte of TARGET
*                           The rest of TARGET is untouched.
*                           TARGET changes from 11111 to 11117

         HLT                Stop the machine
         END                End of the program text

------------------------------
Now that you know how to load, add, and store data, we can build a small complete program. Would you like to see:

* How to write a loop that copies an entire array to a new memory location using LDA and STA?
* How to use ST1 through ST6 to save the values of Index Registers directly to memory?


# INCA
The INCA instruction stands for Increment Accumulator. It is a quick and efficient way to add a number directly to the Accumulator Register (rA) without needing to look up a value in memory first.
Here is the complete guide on how it works and how to use it.
## The Anatomy of INCA
The full syntax for the INCA instruction looks like this:
INCA ADDRESS,I

* ADDRESS: In this instruction, the address field does not represent a memory location. Instead, the literal number you write here is the actual value added to rA.
* ,I (Optional Index Register): If you include an index register, the value inside that register is added to your literal number before incrementing rA.
* No F-Field: You cannot use an F-field specification (like (1:3)) with INCA. It always affects the entire register.

## How INCA Behaves

   1. MIX takes the value currently inside rA.
   2. It takes the literal number from the ADDRESS part of the instruction.
   3. It adds them together algebraically.
   4. The new total is saved back into rA.
   5. Overflow Warning: Just like the ADD instruction, if the result is too large for a MIX word, the internal Overflow Toggle turns on.

------------------------------
## Code Demonstration
Here is a complete MIXAL program. It demonstrates how to use INCA to add a fixed amount, how to use it with a negative number to decrease rA, and how to combine it with an index register.

* --- Define Data Block ---
         ORIG  2000
SAVESPOT CON   0            A place to save our final result

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0

* Demo 1: Basic Increment
         ENTA  10           Load the number 10 into rA
         INCA  5            Add 5 directly to rA (no memory lookup needed)
*                           rA now holds: 10 + 5 = 15

* Demo 2: "Decrementing" using INCA with a Minus Sign
         INCA  -3           Add -3 directly to rA
*                           rA now holds: 15 + (-3) = 12

* Demo 3: Using INCA with an Index Register
         ENT6  20           Set Index Register 6 (rI6) = 20
         INCA  10,6         The amount added is: 10 + (value of rI6)
*                           10 + 20 = 30 is added to rA
*                           rA now holds: 12 + 30 = 42

* Save the result and stop
         STA   SAVESPOT     Save 42 into memory
         HLT                Stop the machine
         END                End of the program text

------------------------------
Now that you know how to use INCA, would you like to see:

* How to use DECA (Decrement Accumulator) to count downwards?
* How to use INC1 through INC6 to update your array indexes inside a loop?


# CMPA
The CMPA instruction stands for Compare Accumulator. Its job is to compare the value currently inside the Accumulator Register (rA) with a value stored in memory.
It does not change the value in rA or memory. Instead, it changes an internal switch inside the MIX computer called the Comparison Indicator. You then use jump instructions (like JE, JG, or JL) to make decisions based on that indicator.
------------------------------
## The Anatomy of CMPA
The full syntax for the CMPA instruction looks like this:
CMPA ADDRESS,I(F)

* ADDRESS: The memory location holding the number you want to compare against rA.
* ,I (Optional Index Register): The register (1 to 6) used to shift the memory address.
* (F) (Optional Field Specification): Tells MIX which specific bytes of the memory word to compare. If left out, it defaults to (0:5) (the whole word).

## How the Comparison Indicator Changes
After running CMPA, the Comparison Indicator is set to one of three states:

* E (Equal): If the value in rA matches the value in memory.
* G (Greater): If the value in rA is larger than the value in memory.
* L (Less): If the value in rA is smaller than the value in memory.

------------------------------
## Code Demonstration
Here is a complete MIXAL program. It sets up a target limit number in memory, loads a value into rA, compares them using CMPA, and jumps to different sections of code depending on the result.

* --- Define Data Block ---
         ORIG  2000         Start data at memory address 2000
LIMIT    CON   50           Our comparison target threshold
RESULT   CON   0            A slot to save a status code

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0

* Step 1: Set up the accumulator
         ENTA  75           Load 75 into rA

* Step 2: Compare rA against our LIMIT in memory
         CMPA  LIMIT        Compares 75 (rA) against 50 (LIMIT)
*                           Since 75 > 50, Indicator is set to G (Greater)

* Step 3: Branch based on the Comparison Indicator
         JL    IS_LESS      Jump if Indicator is L (Less)
         JE    IS_EQUAL     Jump if Indicator is E (Equal)
         JG    IS_GREATER   Jump if Indicator is G (Greater)

* --- Branch Paths ---
IS_LESS  ENTA  1            If less, set rA to 1
         JMP   FINISH       Go to the end

IS_EQUAL ENTA  2            If equal, set rA to 2
         JMP   FINISH       Go to the end

IS_GREATER ENTA 3           If greater, set rA to 3
*                           (The program will take this path)

* --- Wrap Up ---
FINISH   STA   RESULT       Save status code (3) into memory
         HLT                Stop the machine
         END                End of the program text

------------------------------
Now that you know how to compare values, would you like to see:

* How to use CMP1 through CMP6 to compare index registers instead of rA?
* How to use CMPA with partial F-fields to compare specific characters or bytes?


# JMP
The JMP instruction stands for Jump. It is an unconditional jump, which means it forces the MIX computer to break out of its normal top-to-bottom line order and instantly go to a different part of your program. [1, 2, 3] 
Here is the complete guide on how it works and how to use it.
## The Anatomy of JMP
The full syntax for the JMP instruction looks like this:
JMP ADDRESS,I [4] 

* ADDRESS: The label or memory location of the instruction you want to run next.
* ,I (Optional Index Register): If you include an index register, its value is added to the address, allowing you to jump to a dynamic location.
* No F-Field: You cannot use an F-field specification with JMP. [5] 

## The Special Side Effect: rJ Register
When you execute a JMP instruction, the MIX computer automatically updates Index Register 7, which is better known as rJ (the Jump Register).

* Right before jumping, MIX saves the address of the very next line of code into rJ.
* This makes JMP incredibly useful for calling subroutines (functions), because the program will remember exactly where to return when the function finishes. [6, 7] 

------------------------------
## Code Demonstration
Here is a complete MIXAL program. It demonstrates how to use JMP to skip over a section of code entirely, and how to use it to jump back to a loop. [8, 9] 

* --- Define Data Block ---
         ORIG  2000
COUNTER  CON   0            A memory slot to track our loop

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0

* Demo 1: Unconditional Skip
         ENTA  10           Load 10 into rA
         JMP   SKIP_BLOCK   Instantly jump over the next two instructions
         
         INCA  5            This line is skipped!
         INCA  5            This line is skipped too!

SKIP_BLOCK STA COUNTER      rA is still 10, save it to memory

* Demo 2: Using JMP to close a Loop
         ENT1  3            Set Index Register 1 (rI1) = 3

LOOP     DEC1  1            Decrease rI1 by 1
         JZ    FINISH       If rI1 hits 0, jump to FINISH (Conditional)
         
*                           [Your loop work would go here]
         JMP   LOOP         Go right back to the top of the loop (Unconditional)

FINISH   HLT                Stop the machine
         END                End of the program text

------------------------------
Now that you know how to use unconditional jumps, would you like to see:

* How to use rJ and JMP together to create a reusable function/subroutine?
* How conditional jumps like JNE (Jump Not Equal) or JAN (Jump Accumulator Negative) compare to JMP?


[1] [https://faculty.kfupm.edu.sa](https://faculty.kfupm.edu.sa/COE/shazli/coe205/Help/asm_tutorial_07.html)
[2] [https://simplifycpp.org](https://simplifycpp.org/?id=a0879)
[3] [https://www.plantation-productions.com](https://www.plantation-productions.com/Webster/www.artofasm.com/DOS/ch06/CH06-5.html)
[4] [https://collegeek.com](https://collegeek.com/8085_microprocessor/branchinginstruction.html)
[5] [https://collegeek.com](https://collegeek.com/8085_microprocessor/branchinginstruction.html)
[6] [https://www.cs.emory.edu](http://www.cs.emory.edu/~cheung/Courses/255/Syllabus/8-SPARC/call+jmpl.html)
[7] [https://cs.uwaterloo.ca](https://cs.uwaterloo.ca/~plragde/flaneries/FICS2/PRIMP__A_primitive_imperative_language.html)
[8] [https://akash-nasm-tutorial.netlify.app](https://akash-nasm-tutorial.netlify.app/example/jump)
[9] [https://simplifycpp.org](https://simplifycpp.org/?id=a0879)


# JL
The JL instruction stands for Jump Less. It is a conditional jump instruction. It tells the MIX computer to branch to a different part of your program, but only if the internal Comparison Indicator is currently set to L (Less). [1, 2] 
You almost always run a comparison instruction (like CMPA or CMP1) right before using JL.
------------------------------
## The Anatomy of JL
The full syntax for the JL instruction looks like this:
JL ADDRESS,I

* ADDRESS: The destination label or memory location you want to jump to if the condition is met.
* ,I (Optional Index Register): An index register used to shift the destination address.
* No F-Field: You cannot use an F-field specification with JL.

## How JL Behaves

   1. MIX checks the internal Comparison Indicator (which was set by a previous CMP instruction).
   2. If the indicator is L (Less), the computer jumps to the ADDRESS.
   3. If the indicator is E (Equal) or G (Greater), the computer ignores the jump and moves straight to the very next line of code.
   4. The rJ Register: Just like the regular JMP instruction, if the jump happens, MIX saves the address of the next sequential instruction into rJ. [3] 

------------------------------
## Code Demonstration
Here is a complete MIXAL program. It loads a temperature value into the accumulator, compares it to a freezing point threshold, and uses JL to decide whether to activate a freezing-weather alert flag.

* --- Define Data Block ---
         ORIG  2000         Start data at memory address 2000
FREEZING CON   32           The freezing point threshold
TEMP     CON   18           The current recorded temperature
ALERT    CON   0            A flag slot (0 = Normal, 1 = Freezing Alert)

* --- Begin Execution ---
         ORIG  0            Start program at memory address 0

* Step 1: Set up the values
         LDA   TEMP         Load current temperature (18) into rA

* Step 2: Compare rA against our threshold in memory
         CMPA  FREEZING     Compares 18 (rA) against 32 (FREEZING)
*                           Since 18 < 32, Indicator is set to L (Less)

* Step 3: Branch using JL
         JL    COLD_PATH    If Indicator is L, jump to COLD_PATH
*                           (The program will take this jump!)

* --- Normal Path (Skipped if temperature is less than 32) ---
         ENTA  0            Set status to 0 (Normal)
         STA   ALERT        Save status
         JMP   FINISH       Exit program

* --- Cold Path ---
COLD_PATH ENTA 1            Set status to 1 (Alert!)
          STA  ALERT        Save status into memory

* --- Wrap Up ---
FINISH   HLT                Stop the machine
         END                End of the program text

------------------------------
Now that you know how JL reacts to comparisons, would you like to see:

* How to use J1L through J6L to jump if an index register is less than zero directly?
* How to combine JL and JE to handle a "less than or equal to" scenario?


[1] [https://www.plantation-productions.com](https://www.plantation-productions.com/Webster/www.artofasm.com/Linux/HTML/LowLevelControlStructsa2.html)
[2] [https://www.dailymotion.com](https://www.dailymotion.com/video/x9xiy58)
[3] [https://faculty.kfupm.edu.sa](https://faculty.kfupm.edu.sa/COE/shazli/coe205/Help/asm_tutorial_07.html)
