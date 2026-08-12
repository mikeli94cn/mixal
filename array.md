To use MIXAL (MIX Assembly Language) to define an array and visit its elements, you use memory reservation commands and index registers. MIX has special registers called Index Registers (rI1 to rI6) that make walking through arrays very easy. [1] 
Here is the complete guide on how to do this.
## 1. How to Define an Array
You define an array in MIXAL using the EQU or ORIG pseudo-operations to set aside a block of memory.

* Use ORIG to move the location counter.
* This leaves empty words for your array data.
``` mixal
SIZE     EQU   100          Define the array size (100 elements)
ARRAY    ORIG  *+SIZE       Reserve 100 words of memory
```
## 2. How to Visit Array Elements
To visit elements, you load the element's position into an Index Register (like rI1).

* MIX addresses can use an index register to change the target memory location.
* The actual memory address looked up is: Base Address + Register Value.

## Option A: Walking Forward (First to Last)
This method starts at index 0 and counts up to the size of the array. [2, 3] 

* Set the index: Clear your index register to 0.
* Access the item: Use the register next to the array name.
* Step forward: Increase the index register by 1.
* Check the end: Compare the index to your maximum size.
``` mixal
         ENT1  0            Set Index Register 1 (rI1) = 0
LOOP     LDA   ARRAY,1      Load value at (ARRAY + rI1) into rAX
*                           [Your code to process the element goes here]
         INC1  1            Increment rI1 by 1 (move to next element)
         CMP1  SIZE         Compare rI1 to the array size
         JL    LOOP         If rI1 < SIZE, jump back to LOOP
```
## Option B: Walking Backward (Last to First)
This method is often faster and uses fewer lines of code in MIXAL because you can compare directly against zero.

* Set the index: Load the total size into your index register.
* Access the item: Use a negative offset or count down.
* Step backward: Decrease the index register by 1.
* Check the end: Loop until the register hits 0.
``` mixal
         ENT1  SIZE         Set rI1 = 100 (start at the end)
LOOP     LDA   ARRAY-1,1    Load value at (ARRAY - 1 + rI1) into rAX
*                           [Your code to process the element goes here]
         DEC1  1            Decrement rI1 by 1
         J1P   LOOP         If rI1 > 0, jump back to LOOP
```
------------------------------
If you want to practice writing this code, let me know:

* What type of data are you putting in the array?
* Do you want to sum the elements, find the maximum value, or just initialize them to zero?

I can provide the exact code block for your specific goal.

[1] [https://esolangs.org](https://esolangs.org/wiki/MIX_%28Knuth%29)
[2] [https://developer.electricimp.com](https://developer.electricimp.com/squirrel/array/insert)
[3] [https://www.vaia.com](https://www.vaia.com/en-us/textbooks/computer-science/starting-out-with-c-from-control-structures-through-objects-6-edition/chapter-7/problem-54-t-f-to-calculate-the-amount-of-memory-used-by-an-/)

---





To set specific data in your array when you define it, you use the CON (Constant) pseudo-operation. Instead of leaving empty space, CON places a specific number directly into a word of memory.
Here is how to define an array with pre-set data and how to visit its elements. [1] 
## 1. How to Define an Array with Data
You list your numbers one after the other using the CON command. Each CON takes up exactly one word of memory.
``` mixal
* Define a 5-element array with data: 10, 20, 30, 40, 50
ARRAY    CON   10           First element (Index 0)
         CON   20           Second element (Index 1)
         CON   30           Third element (Index 2)
         CON   40           Fourth element (Index 3)
         CON   50           Fifth element (Index 4)
SIZE     EQU   5            Keep track of the total count
```
## 2. How to Visit the Elements
Visiting this array works exactly the same way as an empty one. You use an Index Register to step through the memory locations.
Here is a complete, runnable example that loops through the data we just defined and adds all the numbers together. [2] 
``` mixal
* Initialize registers
         ENT1  0            Set Index Register 1 (rI1) = 0
         ENTA  0            Clear Accumulator (rA) = 0 (this holds our sum)

* Loop through the array
LOOP     ADD   ARRAY,1      Add the element at (ARRAY + rI1) to rA
         INC1  1            Move index to the next element
         CMP1  SIZE         Check if we hit the end
         JL    LOOP         If rI1 < 5, repeat the loop

* After the loop finishes, rA contains the total sum (150)
         HLT                Stop the MIX machine
```
------------------------------
Now that your data is set up, let me know:

* Do you need help modifying the values inside the loop (like doubling each number)?
* Would you like to see how to find the largest number in this specific data set?

I can write out the exact logic for your next step.

[1] [https://www.facebook.com](https://www.facebook.com/groups/companion/posts/3589051031313221/)
[2] [https://www.freecodecamp.org](https://www.freecodecamp.org/news/javascript-map-reduce-and-filter-explained-with-examples/)
