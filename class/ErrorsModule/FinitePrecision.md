## Finite Precision

A transcendental number like $\pi$ has an infinite number of digits.  Clearly, a computer is capable of only storing a finite number of these digits.  We are familar with the idea that rational numbers like $1/3 = 0.333333...$ in base 10, or $0.0101010101...$ in binary, must typically be truncated.  However, it is important to remember that computers store numbers in base 2 (binary) so that numbers like $1/10 = 0.1$ in base 10, is $0.000110011001100...$ have an infinite number of digits in binary and will have to be truncated when represented in a computer.  In this section we will discuss the impact of the computer's finite precision on numerical algorithms.

### Precision versus Accuracy

It is important to distinguish what we mean by precision versus what we mean by accuracy.  In numerical computation we typically have 32 or 64 binary digits of *precision* in our calculations (which translates to roughly 8 or 16 decimal digits).  This does not mean we have anything like this amount of accuracy (as measured by absolute or relative errors).  In high school you may have been taught to discard any digits that were not accurate by keeping only significant digits in calculations.  This philosophy places value only on accuracy and none on precision.  However, there is also value in precision even if it far exceeds the accuracy of the numbers involved.

An example of the value of precision, even in the absense of accuracy, was in the Hubble space telescope (see <https://www.nasa.gov/content/hubbles-mirror-flaw>).  The Hubble was a complete embarassment for NASA when it was first launched.  It was ground to the wrong specifications due to a calibration error.  As a result, it produced fuzzy images.  Fortunately, it was ground to a very high precision (with poor accuracy).  The high precision allowed NASA to install corrective lenses to cancel the error (the poor accuracy) resulting in the amazing clear images you are now familar with.

As we will discuss below, the finite precision of floating-point numbers can result in a loss of accuracy as they lead to errors that can not just propogate but be significantly magnified by arithmetic operations.  The easiest, but not necessarily the best, way to deal with these errors is to increase the number of digits used to store numbers.  Over the years, this has lead to the number of binary digits used to store numbers increasing from 16-bit, to 32-bit, and then to 64-bit.  In addition, typically CPUS use 80-bit registers (at time of writing in 2022) to store intermediate results for 64-bit floating point calculations.  However, many computationally intensive calculations are now moving to GPU-computing (graphics processing unit) which are either limited to 32-bit floating point numbers, or perform significantly faster in 32-bit rather than 64-bit mode.  In addition, many of the typical "shortcuts" done for performance can often lead to a preproducability problem (ie. the program may give slightly different results from one run to the next, see <https://www.intel.com/content/dam/develop/public/us/en/documents/fp-consistency-121918.pdf>).  The best way to avoid these problems is to have a firm understanding of the origin of errors originating from finite precision arithmetic and design your algorithm and code in such a way to avoid them, or at least minimize their effects.

### Under/Over flow

As we mentioned, numbers have a finite precision in order to be stored in a computer.  Different types of numbers have different precision.  The table below gives the number of digits and range of several types of numerical types.

| mathematical type | C-style code type | number of binary digits | range        | decimal precision |
|-------------------|-------------------|-------------------------|--------------|-----------|
| integer           | 'short'           | 16-bit                  | $-2^{15}=-32 768$ to $2^{15}-1=32 767$ | - |
|                   | 'int'             | 32-bit                  | $-2^{32}=-2 147 483 648$ to $2^{32}-1$ | - |
|                   | 'long'            | 64-bit                  | $-2^{64}$ to $2^{63}-1$ | - |
|-------------------|-------------------|-------------------------|--------------|------------|
| real number       | 'float'           | 32-bit                  | '1.2E-38' to '3.4E+38' | 6 digit|
|                   | 'double'          | 64-bit                  | '2.3E-308' to '1.7E+308' | 15 digit |
|                   | 'long double'     | 80-bit                  | '3.4E-4932' to '1.1E+4932' | 19 digit |
