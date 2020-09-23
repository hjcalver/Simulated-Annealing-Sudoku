## Simulated-Annealing-Sudoku

# Introduction
Example of a simulated annealing solver written in c++. 

# Algorithm Overview

1) Fill out each sub-square 'block', with random numbers such that each is a subsolution (contains numbers 1-9 only).  
2) The problem state is scored by counting the number of repeats in each row and each collumn: iff puzzle solved score=0.  
3) We create a new problem state by randomly selecting a block, and randomly swapping two of the unfixed elements (not clues).   
4) Score the new state and find the difference with old score;  
        if the score improves accept the new state.  
        else if the state doesn't improve use probablilty function P=1/(1+e^(score_difference/T)) where T is the global temperature parameter.  
                if random U[0,1] < P accept new state, else reject.  
5) Lower the global temp T after each iteration.  
6) If score doesn't improve set number of times, 'reheat' by returning T to start Temp.  

# Performance
The implementation has been tested on puzzles of varying complexity. It has so far performed well even on puzzles with few clues, taking around 0.3s to find a solution (running on an i5 2.7Ghz macbook). Occasionally the solver would end up stuck in a large local minima and fail to improve the score sequentially, to solve this 'reheating' was implemented.

# Code/algorithm optimisation 
The most important steps for improving runtime are; selecting the random elements to switch, and computing the new score:  
To select random elements the main problem is stored as an array for fast access. The coordinates of movable elements are stored as an array of vector pairs. The program can access the set of coordiantes for each block quickly, a fischer yates shuffle is then used to pull two sets of coordiantes from the vector.
The scoring process was slightly optimised by only computing the score change in the rows/collumns being changed. Further if the two elements were in the same row or collumn that score was not calculated.
