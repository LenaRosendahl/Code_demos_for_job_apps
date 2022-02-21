TIME INDEPENDENT SCHRODINGER EQUATION (TISE) SOLVER:
AUTHOR: Morgan (Lena) Rosendahl 
PURPOSE: This repo serves two purposes:
1: Provide prospective employers with an accessibly sized sample of Python coding that both illustrates my work process and offers a standalone piece of code that solves a non-trivial problem
2: To solve that non-trivial problem: finding all bound states of the TISE for an MPMW-style square well landscape with any number of wells, their respective PDFs, and the probabilities of 
finding a particle in each well at each energy (MIEs). This may be done for either an infinite linear landsacpe or a ring landscape. 

ABOUT SOLVING THIS PROBLEM:
Outside of a couple of landscapes, the TISE does not have analytic solutions. A variety of numerical methods exist to solve it, however, many code implementations of these methods only solve
 for the ground state and may not provide energies, states, and PDFs. Very few provide MIEs. All of this information is required for implementing models of decision-making using the MPMW Framework
(see Rosendahl & Cohen 2021, Rosendahl & Cohen 2022). To address this problem, I have implemented the Matrix Numerov Method described by Pillai et al.

The final results, as described above, may be calculated by running solve_tise_linear_fun.ipynb or solve_tise_ring_fun.ipynb

The process is as follows:
We define a potential landscape of square wells, each of which is set into a constant background potential. These wells may have any depth, width, and non-zero separation. User provides the number of wells
and arrays of their depths, widths, and seaparations. From these, the landscape is defined and embedded in an array of the appropriate size (as adapted from Pillai et al for modeling purposes) in
  gen_landsape_ring. To generate a properly sized landscape, we must consider both the resolution of the landscape (dx), which is calculated from the minimum de broglie wavelenght of the system, and 
the appropriate width on either side of the landscape of interest to allow for the solutions to decay (as required for imitating the infinite linear landscape).

After defining the potential, we convert the TISE from its traditional form into its matrix form and solve the system for all bound states. Again, we follow the methods outlined in Pillai et al. 

The eigen energies are the eigenvalues below the background value (max(V(x))) and the pre-normalized states are the corresponding eigenvectors. 

As the MPMW is interested primarily in probabilities, we normalize the PDFs rather than the states themselves. PDFs are calculated by taking the piecewise square of each state across its entire breadth.

We calculate the MIEs by integrating the normalized PDFs (using the trapezoidal method, as these are technically not continuous vectors) between the bounds of each well. 

The final cell in solve_tise_linear_fun.ipynb and solve_tise_ring_fun.ipynb provide plots of some of the PDFs for sample landscapes. These can be toyed with, however, not all landscapes will include 6 bound
states, so the plotting (which assumes a minimum of 6 bound states) may return an error if parameters are changed.

ABOUT THE CODING PROCESS USED TO BUILD THIS REPO:
This problem can be broken down into building out the potential and solving the TISE. For each part, I built a piece of standalone code, verified that that code worked as intended, and then converted it into
a function. 

Although incorporating user defined functions may be handled by building an environment, it is the intention of this repo to make all code as readable and as accessible as possible with minimal work on the 
part of the user. This code is meant to return useful results, but is not meant to be used in actual models. Rather, it is intended to be viewed as a demonstration of proficiency in Python that can be used
to solve real quantitative problems.

SUGGESTIONS FOR FUTURE WORK:
1: Streamline this code base to take a user input that defines the need for a linear or ring landscape and solves the appropriate problem (as they are very similar)
2: Provide a simple validation metric for resolution that checks that the number of bound states is in keeping with the expected number
	2a: Should a solution return an inappropriate number of bound states, allow the user to increase resolution (dx) on the landscape and attempt to run the code again
3: Use an eigensolver that capitalizes on the tridiagonal nature of the system used here to create greater efficiency.

REFERENCES:
1: Pillai, Mohandas, Joshua Goglio, and Thad G. Walker. "Matrix Numerov method for solving Schrödinger’s equation." American Journal of Physics 80.11 (2012): 1017-1019.
2: Rosendahl, Morgan L., and Jonathan Cohen. "A Discrete Formulation of Two Alternative Forced Choice Decision Dynamics Derived from a Double-Well Quantum Landscape." bioRxiv (2021).
3: Rosendahl, Morgan L., and Jonathan D. Cohen. "A Quantum Cognitive Perspective on Control and Arousal." Retrieved from psyarxiv. com/ngwy7 (2022).