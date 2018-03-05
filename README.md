# LingSieve
CUDA (GPU) implementation of the sieve of Eratosthenes.


Key components used to make it really fast:
 - segmented sieve of Eratosthenes
 - sieve in shared memory
 - bucket concept
 - wheel concept
 - compact layout of primes to be sieved - one odd number per bit
 - reuse all data structurs again and again - no malloc, no free hence no data leaks and init time can be neglected
 - different sieving method for small, medium, large and very large numbers


The program LingSieve is designed for long duration tests (LDT), to handle large quantities of data. That is what GPU computing is all about. It makes no sense to to use the GPU with small sieve jobs - programs running on CPU are the right joice for this kind of jobs.
The granularity is 1.000.000.000 = 10^9 = 1" = 1 cycle, that is the smallest quantitie to be sieved. It can be startet with multiples of 1", and the sieve width is also in multiples of 1".


Prerequiste
===========

 - NVIDIA graphics card - works best with compute capabilities 3.7, 5.2, 6.1 and 7.0
   because of available quantities of registers and shared memory
 - a 64-bit host application and non-embedded operating system (Linux, Windows, macOS)
 - GPU Mmemory requirement: 1832MB .. 2412MB depending on the range to be sieved


Usage
=====

  1) start without parameter - it will use initial values - StartValue (0), SieveWidth (100") and start a LDT
  2) if there is a valid Result file it will use the last entry as StartValue, SieveWidth (100") and start a LDT
  3) if there is a user input it will use it instead of 1) or 2)


  Open a Command Prompt and run:

  - LingSieve -verbose -bench		- // - to get on overview of the capabilities of your graphics card
  - LingSieve				- // - Start LDT from 0 or from the last entry of file Result.txt if there is any
  - LingSieve 0			          // Count the primes from 0 .. 10^11
  - LingSieve 0 -s1		        // Count the primes from 0 .. 10^9
  - LingSieve 0 -s10		      // Count the primes from 0 .. 10^10
  - LingSieve 1000000		      // Count the primes from 10^15 .. 10^15+10^11
  - LingSieve 100000000 -s1000	// Count the primes from 10^17 .. 10^17+10^12
  - LingSieve /?			        // Display help text
