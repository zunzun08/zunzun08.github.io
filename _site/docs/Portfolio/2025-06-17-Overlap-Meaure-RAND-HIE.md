### Prerequisite: Overlap Measure

# Introduction

The overlap measure provides us a way to conceretly measure changes between a population before and after a treatment. What makes the overlap measure so powerful is that it is a method of comparison between populations without the use of a p-value that also gives a value of the percent of individuals who change after having a treatment applied. Overlap, therefore, does not suffer from the same downsides as the p-value, mainly Overlap is not:

- Vulnerable to p-hacking
- A probability that can be proven to be false

While I do not believe the overlap measure should be a direct replacement of the p-value, nor do I believe that was the purpose of the incepetion of the overlap measure; by removing the p-value from our statisical analysis we force ourselves to recreate the strength of the validity "statistically significant"results have with a far more data driven approach. The overlap merely carries with it a numerical ouput of the distirbutional similiarity between the before and after treatment distributions.

The goal of this article is thus a practical application of the overlap measure and discovering insights into datasets where the p-value dound results that were not statistically significant but individuals who underwent a treatment did still behave differently than they did before the treatment.


## Introduction to RAND HIE


