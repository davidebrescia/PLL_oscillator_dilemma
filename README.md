# PLL Oscillator Dilemma

In the CDCE706 PLL oscillator, the output frequency is given by this formula:

f = 100 * N / (M * P)  

where N, M, P are integers, and:  
1 ≤ N ≤ 4095  
1 ≤ M ≤ 511  
1 ≤ P ≤ 127  

## Problem
Given a user defined frequency "f_user", find the best triplet (N, M, P) that minimizes |f - f_user|.  \
In practical terms, the user wants a desired exact frequency, for example 100.8 MHz. Is it possibile to obtain that exact frequency, by finding some integer N, M and P? If not, what is the best N, M, P that better approximate the output frequency? 

## Solution
My algorithm is very fast (100-150 ms).  
I will now explain the algorithm, but just for this time, don't get used to it :)      
1) We start from the array of possible Q values (Q := MxP), saved before in memory. (The array connects every possible value of Q with a single pair M, P that generates it), we scan this Array from the lowest Q.
2) We calculate N_theoretical = Q * f_user / 100. (It's the N that would return exactly f_user, it's a float though).
3) We approximate N_theoretical to the nearest integer "N_neighbor", if N_neighbor = N_theoretical we have already found the best (N,M,P) and the algorithm stops, otherwise we calculate the quantity |N_theoretical - N_neighbor|/Q and save in an array "errors_array".
4) We find the minimum of errors_array.
