# LingSieve
CUDA (GPU) implementation of the sieve of Eratosthenes.


Key components used to make it really fast:
 - segmented sieve of Eratosthenes
 - sieve in shared memory
 - bucket concept
 - wheel concept
 - compact layout of primes to be sieved - one odd number per bit
 - reuse all data structures again and again - no malloc, no free hence no memory leaks and init time can be neglected
 - different sieving method for small, medium, large and very large numbers



The program LingSieve counts primes and is designed for long duration tests (LDT), to handle large quantities of data. That is what GPU computing is all about. It makes no sense to use the GPU for small sieve jobs.
The granularity is 1.000.000.000 = 10^9 = 1" = 1 cycle, that is the smallest quantity to be sieved. It can be started with multiples of 1", and the sieve width is also in multiples of 1". It can sieve short of 2^64 (1,8446744x10^19).


 
Benchmark and HW Scaling
========================

The GPU HW will scale the program accordingly, there is no need to configure anything.


Range | Count | GTX 1080 Ti | GTX 1080 | GTX 1070 | GTX 1060 | GTX 1050
----- | ----- | ----------- | -------- | -------- | -------- | --------
0 … 10^11	| 4.118.054.813	| | 0,60s
0 … 10^12	| 37.607.912.018	| | 8,31s
0 … 10^13	| 346.065.536.839	| | 103s
0 … 10^14	| 3.204.941.750.802	| | 1231s
10^12 + 10^11	| 3.612.791.400	| | 0,90s
10^13 + 10^11	| 3.340.141.707	| | 1,08s
10^14 + 10^11	| 3.102.063.927	| | 1,26s
10^15 + 10^11	| 2.895.317.534	| | 1,44s
10^16 + 10^11	| 2.714.336.584	| | 1,62s
10^17 + 10^11	| 2.554.712.095	| | 1,81s
10^18 + 10^11	| 2.412.731.214	| | 2,08s
10^19 + 10^11	| 2.285.693.139	| | 2,64s
1,4x10^19 + 10^11	| 2.268.304.926	| | 2,81s
1,6x10^19 + 10^11	| 2.261.487.864	| | 2,84s
1,8x10^19 + 10^11	| 2.255.482.326	| | 2,91s

This is the fastest implementation of the sieve of Eratosthenes on a GPU to the best of my knowledge.


Prerequisite
============

 - NVIDIA graphics card - works best with compute capabilities 3.7, 5.2, 6.1 and 7.0
   because of available quantities of registers and shared memory
 - a 64-bit host application and non-embedded operating system (Linux, Windows, macOS)
 - GPU memory: 1832MB .. 2412MB depending on the range to be sieved
 
 
Binary
======
The available binaries have been compiled for 64bit Windows and NVIDIA GPUs with compute capabilities 3.7 and higher.


Usage
=====

  Open a Command Prompt and run the program LingSieve:
  
  1) start without parameter - it will use initial values - StartValue (0), segment size (100") and start a LDT
  2) if there is a valid Result file it will use the last entry as StartValue, segment size (100") and start a LDT
  3) if there is a user input it will use it instead of 1) or 2)


  
  
Examples            | Comment
------------------- | --------
  LingSieve -verbose -bench		| to get on overview of the capabilities of your graphics card
  LingSieve				             | Start LDT from 0 or from the last entry of file Result.txt if there is any
  LingSieve 0			          | Count the primes from 0 .. 10^11
  LingSieve 0 -s1		        | Count the primes from 0 .. 10^9
  LingSieve 0 -s10		      | Count the primes from 0 .. 10^10
  LingSieve 1000000		      | Count the primes from 10^15 .. 10^15+10^11
  LingSieve 100000000 -s1000	| Count the primes from 10^17 .. 10^17+10^12
  LingSieve /?			        | Display help text
  
  
  
  Status
  ======
  
  I've sieved 6,8x10^16 numbers in various ranges without any errors, that is 68.000.000 cycles. Once I run it for more than 60 hours continuously without any problems, it just works as it is supposed to. The results were compared with the extensive prime sieve tables of Tomás Oliveira e Silva.
  
  
  Basic development is finished.
  I may append it with the following features in the future:
  - support of multi GPUs
  - sieve up to 10^20
