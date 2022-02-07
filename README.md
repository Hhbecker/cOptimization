# cOptimization



This repository contains my work using R. Bryant and D. O'Hallaron's C optimization code template developed at Carnegie Mellon University. This template will allow me to test out the effect of different code omptimization techniques on the clock time and number of cycles needed by the processor to execute the code. 

The objective is to show how program performance can be improved by using only a small menu of optimization techniques.

to do 
* understand how rotate and smooth work 
* take notes on the class slides 
* start development on the seas shell 
* try out different techniques, and then describe which techniques were effective, or not effective, and why or why not

Where's the line between readability and optimization?


If you examine the kernels.c code you will find that I am using the timestamp counter ( rdtsc ) and usage time. Both of these have problems when it comes to accurate measurements – specifically, they are affected by other system tasks. (So you will may get weird times sometimes; including differing times each time you run the project. THIS is why the report is very important.)

Both code and a report 

The task is to optimize memory intensive code. While the speed of processors is increeasing, the speed of memory devices is increasing at a much slower rate which has resulted in a significant speed mismatch between processors and memory devices. This means processors will often have to sit idlly while they wait for a necessary piece of data to be retrieved from memory to continue execution of an instruction. 


The code in this project is for image processing. There are two functions that I will try to optimize:
1. naive_rotate = rotating an image by 90º
2. naive_smooth = smoothing or blurring an image by averaging pixel values using adjacent pixels 


Images are stored as 2D matrices

Rotate is implemented as the combination of a:
1. Transpose operation
2. Exchange rows oepration


The only file I will be modifying is Kernels.c 

    typedef struct {
    unsigned short red;   /* R value */
    unsigned short green; /* G value */
    unsigned short blue;  /* B value */
    } pixel;

As can be seen, RGB values have 16-bit representations (“16-bit color”).


#### Function-like macros 

These are predefined functions that the C preprocessor will recognize.

An example of a function-like macro:
    #define circleArea(r) (3.1415*(r)*(r))
Every time the preprocessor encounters circleArea(argument), it replaces it with (3.1415*(argument)*(argument))

An image I is represented as a one-dimensional array of pixels, where the (i, j)th pixel is I[RIDX(i,j,n)]. Here n is the dimension of the image matrix, and RIDX is a macro defined as follows:
#define RIDX(i,j,n) ((i)*(n)+(j))

### Performance Measures

Our main performance measure is the speedup of optimized code (i.e., your versions) to the naive (original) version. The driver will output the performance of both the naive method and your method for each of the algorithms (smooth and rotate). We are using two system utilities to measure the performance – (1) the number of (processor) cycles it takes to run for an image of size N × N for different values of N and (2) the execution time in milliseconds. If you examine the kernels.c code you will find that I am using the timestamp counter ( rdtsc ) and usage time. Both of these have problems when it comes to accurate measurements – specifically, they are affected by other system tasks. (So you will may get weird times sometimes; including differing times each time you run the project. THIS is why the report is very important.)

The improvement in performance is usually measured one of two ways: (a) the difference in the old and new times as a percentage of the old time or (b) the ratio of the old execution time to the new execution time (i.e, the speedup which signified how much faster). While metric (a) is the more classical approach, in this project you can use the simpler metric of ratio.
• Speedup S = Tnaive/Topt where Tnaive is the original time and Topt is the optimized time. For example if Tnaive = 600 and Topt = 400 then S = 600/400 = 1.5 which we refer to (in this project) as a 50% improvement. So a x% improvement in this context means Tnaive/Topt = 1.x.
• Theratios(speedups)oftheexecutiontimeoftheoptimizedimplementationoverthenaiveonewillbeusedto compute your grade for your implementation.
(The classic method of measuring percentage improvement would be σ = (Tnaive − Topt)/Tnaive.) Assumptions
To make life easier, you can assume that N is a multiple of 32. Your code must run correctly for all such values of N , but we will measure its performance only for large sizes (of 512 or greater).


* This analysis, which should be provided in the report, is as important as getting the code to run - and your grade will be based off both the report and the code. The report will play a very large role in your grade – if you have not explained your logic correctly, your grade will suffer even if your code works. 
* You must use the optimization techniques covered in the lectures – any other techniques you used must be explained (i.e., what they are, why they work and proof of correctness for why they work

* The minimum improvement we expect (for a passing grade) is 30% – you will find that you can get much higher improvements by trying out different optimizations. The report will be used to gauge how well you were able to analyze why the optimizations worked (for example, simply stating ”improved locality” is not an in-depth analysis and will be considered as no explanation – you will need to explain how or why the locality was improved).

## Grading
#### Correctness: 
* You will get NO CREDIT for buggy code that causes the driver to complain! This includes code that correctly operates on the test sizes, but incorrectly on image matrices of other sizes. As mentioned earlier, you may assume that the image dimension is a multiple of 32.
#### Performance: 
* In terms of the number of processor cycles, and/or time, taken by your code. The minimum performance improvement (speedup) that your code must achieve for each function is 30% (specified earlier in this document
* Your solutions must provide at least this speedup to get a passing grade on this project.
* Report: Your report must (for each of the two functions smooth and rotate) describe the optimizations you used and why you think they will improve the performance. 
* Remember that this final project is in lieu of a required final exam – so we expect you to spend enough time preparing a report with a thorough analysis. 

At a minimum, your report must describe:
* (i) the optimization techniques you tried (you may end up using only a subset of these in the code that performs best) for each algorithm (smooth and rotate)
* (ii) the set of optimizations you tried and how effective they were.
*  (iii) provide a complete table of your performance results (i.e., a table showing how much better you did compared to the naive implementations – the performance improvement in speed). Simply including the output of the program does NOT constitute a report!
*  (iv) Which optimizations gave you the best performance and why you think they improved the perfor- mance. If some of the optimizations did not provide much of an improvement then explain why they did not.
* If you chose a specific technique, or a value for a parameter, over another technique your report should provide your rationale (or experimental results, if that was how you reached your decision) for these choices. We will give you (significant) partial credit if your report is correct but your implementations do not provide the minimum speedups we require.


Define these terms:
Some Machine-Independent Optimizations: 
Dataflow Analysis and Optimizations
Constant folding
Copy propagation
Elimination of common subexpression
Dead code elimination
x = 1 
x = b + c or if x is not referred to at all Saves one instruction…reduce IC

Code motion
Strength reduction
Function/Procedure inlining
Improving memory locality

#### Loop invariants:
 A loop invariant is a property of a program loop that is true before (and after) each iteration. If a loop invariant is calculated in each iteration it is best to remove that calculation from the loop itself. Calculate the loop invariant once before the loop begins, store it as a variable, and pass that into the loop.

#### Strength Reduction:
• Replace costly operation with simpler one
• Shift, add instead of multiply or divide
16*x --> x << 4
o Utility is machine dependent (relative cost of multiplication vs addition depends on hardware/ISA)
a : = b*17 a: = (b<<4) + b


#### Function inlining 
 Create activation records, allocate memory
• Manipulate stack and frame pointers
•Instructions to Push arguments to stack
•Instructions to Push frame pointer, return addr.
•Execute instructions of function
•Instructions to Pop return value, reset frame pointer, pop
return address
•The bookkeeping instructions are essentially an
“overhead”
• They do not do the work of the function


#### Locality
The Principle of Locality: programs tend to reuse data and instructions near those they
have used recently, or that were recently referenced themselves.

• Temporal locality: Recently referenced items are likely to be
referenced in the near future.
• Spatial locality: Items with nearby addresses tend to be
referenced close together in time.

Column major vs row major order
* 2D arrays and matrices are stored as 1D arrays in Row Major order in C
* to maximize cache hits you want to access elements sequentially and in row major order


• Merging Arrays: improve spatial locality by single array of compound
elements vs. 2 arrays
• Loop Interchange: change nesting of loops to access data in order
stored in memory
• Loop Fusion: Combine 2 independent loops that have same looping
and some variables overlap
• Blocking: Improve temporal locality by accessing “blocks” of data
repeatedly vs. going down whole columns or rows

    /* Before:*/
    int tag[SIZE]
    int byte1[SIZE]
    int byte2[SIZE]
    int dirty[size]

    /* After */
    struct merge {
    int tag;
    int byte1;
    int byte2;
    int dirty;
    }

struct merge cache_block_entry[SIZE]
An array of these structs which each hold a single element 
A single struct of arrays would do the same thign I believe




Explain how the image matrix is represented as a 1D array 
Explain how a clockwise vs counterclockwise 90º rotation is achieved with the 1D array 

Our focus is on the user CPU time. We are interested in comparing the time the processor spends executing our lines of code specifically. Time spent on I/O and other system routines is not relevant to our comparison. 

Memory access time optimization: How to arrange data/instructions so that we have as few cache misses as possible.



What is a chache? 
What are the Li vs Ld caches?
What is cache size vs block size?
 

    getconf WORD_BIT
    > 32

    arch
    > x86_64

    lscpu
    >Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                80
On-line CPU(s) list:   0-79
Thread(s) per core:    2
Core(s) per socket:    20
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Gold 6138 CPU @ 2.00GHz
Stepping:              4
CPU MHz:               2000.000
BogoMIPS:              4000.00
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              28160K


x86_64 ISA has 64 byte cache block size 

if the cache is 32KilaBytes that's 32k/64 blocks 

if the array stores integers which are each 32 bits (4 bytes) in C then each block stores 64/4= 16 integers in a block

if a block is loaded with the first cell of a 2D array the next 16 cells in that row will be added to the cache.

What is the difference between: 
L1d = L1 data cache
L1i = L1 instruction cache
L2
L3 
caches 

Princeton vs Harvard vs Von Neumman architecture 


Interestingly, malloc is never used to create space for the pixel struct when storing both the original and modified images. This means the image arrays are stored on the stack. As we know the stack grows from higher to lower memory, therefore, if a cache block was created starting with the first index of the array the rest of the cache block would be filled by contents of memory that were not part of the array because the array values after the first cell would be stored in memory at lower addresses than the address of the first cell.


Show image of stack array vs heap array. Count down in hex for practice 
