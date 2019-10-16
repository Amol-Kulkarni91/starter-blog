---
title: Hello World!
date: 2019-10-16T02:13:00.000Z
image: /images/hello_world.jpeg
tags:
  - Cecil
  - SSG
draft: false
---
For my first post, I have decided to do something that every programer learns in their introductory programming course. That's right; this is going to be a post on writing a 'Hello World!' program in python. However, writing such a program in python barely takes 10 seconds depending on your typing speed instead let's write an evolutionary algorithm that will spell out "Hello World!" for us. Before we jump into coding; let's have a brief introduction to the genetic algorithm.

## What are Genetic Algorithms?

Genetic algorithms are tools that are used to find good sometimes even optimal solutions to problems that have billions of solutions. They were proposed by John Holland, his students and David E. Goldberg. Genetic algorithms encode a potential solution to a chromosome like data structure and apply recombination operations to preserve critical information. The basic principle on which genetic algorithm works reflects the process of natural selection.

![](/images/ga_1.jpg)

The process of natural selection starts with the selection of fittest individuals from a population. They produce offspring which inherit the characteristics of the parents and will be added to the next generation. If parents have better fitness, their offspring will be better than parents and have a better chance at surviving. This process keeps on iterating and at the end, a generation with the fittest individuals will be found.

![](/images/flow.jpg)

Genetic algorithms vary in their structure based on their purpose, but all of them share a few common components. The algorithm begins with initializing a population of individuals using default random values. Then, it runs each member of that population through a fitness function. Then the fittest member of the population is selected to reproduce, then the evaluations and reproduction process are repeated until the termination condition is realized. At termination, the algorithm presents a best member of the population. 

#### Genes

Genetic algorithm needs a gene set to build upto the target variable. In this particular case, the genset will be the alphabets from a-z in both cases. Space, period, and exclamation (!) symbol are also included. 

```python
import random  # To generate random samples
import datetime # To display the time taken to arrive at the solution


geneSet = " abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!."
target = "Hello World!"
```

#### Generate Parents
Although, the section is titled "Generate Parents" we are essentially initializing a population.

```python
# Function to generate random strings from the gene set. 
# It takes in length as a parameter and returns the list of strings.

def generate_parent(length):
    genes = []               # Empty list to add the generated parents

# Run the loop until length of the genes list is less than the specified
# length parameter

    while len(genes) < length:

# Select the minimum of (length - len(genes)) and len(geneSet) 
# and assign it to sampleSize 

        sampleSize = min(length - len(genes), len(geneSet))

# random.sample returns a list of strings from geneSet equal to
# length of sample size. Add the elements to the empty list.
  
        genes.extend(random.sample(geneSet, sampleSize))
    return ''.join(genes)
```

#### Fitness Function

The fitness function provides feedback to guide the algorithm towards a solution. In this case, the fitness value is the total number of letters in the same position as that of the target variable.

```python
# Function that adds 1 incase the letters in guess and target match in 
# position and case.

def get_fitness(guess):
    return sum(1 for expected, actual in zip(target, guess) if expected == actual)
```

#### Mutation

In certain new offspring formed, some of their genes can be subjected to a mutation with a low random probability. This implies that some of the bits in the bit string can be flipped. Mutation occurs to maintain diversity within the population and prevent premature convergence.

The following implementation converts the parent string to an array with `list(parent)`, then replaces 1 letter in the array with a randomly selected one from `geneSet` and finally recombines the result into a string with `''.join(childGenes)`

```python
def mutate(parent):
    index = random.randrange(0, len(parent))
    childGenes = list(parent)
    newGene, alternate = random.sample(geneSet, 2)
    childGenes[index] = alternate if newGene == childGenes[index] else newGene
    return ''.join(childGenes)
```

This implementation uses an alternate replacement if the randomly selected `newGene` is the same as the one it is supposed to replace, which can prevent a significant number of wasted guesses.

#### Monitoring Progress

It's important to monitor progress so that the algorithm can be stopped if it gets stuck. Having a visual representation of the gene sequence, which may not be the literal gene sequence, is often critical to identifying what works and what does not so that the algorithm can be improved.

```python
def display(guess):
    timeDiff = datetime.datetime.now() - startTime
    fitness = get_fitness(guess)
    print("{0}\t{1}\t{2}".format(guess, fitness, str(timeDiff)))
```

#### Main Body

The main program begins by initializing `bestParent` to a random sequence of letters and calling the display function. The heart of the genetic algorithm is a loop that:

* generates a random string of letters
* requests the fitness for that guess, then
* compares fitness to that of the best guess, and
* keeps the guess with better fitness

This cycle repeats until a stop condition occurs, in this case when all the letters in the guess match those in the target.

```python
# To reproduce the same result, we call the random.seed function.
random.seed(1)
startTime = datetime.datetime.now() # Record the start time
bestParent = generate_parent(len(target)) 
bestFitness = get_fitness(bestParent)
display(bestParent)


while True:
    child = mutate(bestParent)
    childFitness = get_fitness(child)
    if bestFitness >= childFitness:
        continue
    display(child)
    if childFitness >= len(bestParent):
        break
    bestFitness = childFitness
    bestParent = child
```

#### Result:

```output
PwYXVxqghGja	0	0:00:00.001003
HwYXVxqghGja	1	0:00:00.001003
HwYXVxqghlja	2	0:00:00.007858
HwYXVxqghlj!	3	0:00:00.011940
HwYXV qghlj!	4	0:00:00.024247
HwYXo qghlj!	5	0:00:00.035484
HwYXo qghld!	6	0:00:00.037479
HwYXo qgrld!	7	0:00:00.040592
HwYXo Wgrld!	8	0:00:00.045106
HwlXo Wgrld!	9	0:00:00.057195
HwlXo World!	10	0:00:00.075456
Hwllo World!	11	0:00:00.079659
Hello World!	12	0:00:00.145945
```
