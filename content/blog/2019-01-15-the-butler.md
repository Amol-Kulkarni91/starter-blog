---
title: Hello World!
date: 2019-09-24T11:37:00.000Z
image: /images/hello_world.jpeg
tags:
  - Cecil
  - SSG
draft: false
---
For my first post, I decided to do something that every programer learns in their introductory programming course. That's right; this is going to be a post on writing a 'Hello World!' program in python. However, writing such a program in python barely takes 10 seconds depending on your typing speed instead let's write an evolutionary algorithm that will spell out "Hello World!". Before we jump into coding; let's have a brief introduction to the genetic algorithm.

## What are Genetic Algorithms?

Genetic algorithms are tools that are used to find good sometimes even optimal solutions to problems that have billions of solutions. They were proposed by John Holland, his students and David E. Goldberg. Genetic algorithms encode a potential solution to a chromosome like data structure and apply recombination operations to preserve critical information. The basic principle on which genetic algorithm works reflects the process of natural selection.

![](/images/ga_1.jpg)

The process of natural selection starts with the selection of fittest individuals from a population. They produce offspring which inherit the characteristics of the parents and will be added to the next generation. If parents have better fitness, their offspring will be better than parents and have a better chance at surviving. This process keeps on iterating and at the end, a generation with the fittest individuals will be found.


#### Genes 

```python
import random 
import datetime

geneSet = " abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!."
target = "Hello World!"
```

#### Generate Parents

```python
def generate_parent(length):
    genes = []
    while len(genes) < length:
        sampleSize = min(length - len(genes), len(geneSet))
        genes.extend(random.sample(geneSet, sampleSize))
    return ''.join(genes)

```

#### Fitness Function

```python

def get_fitness(guess):
    return sum(1 for expected, actual in zip(target, guess) if expected == actual)
```

#### Mutation


```python

def mutate(parent):
    index = random.randrange(0, len(parent))
    childGenes = list(parent)
    newGene, alternate = random.sample(geneSet, 2)
    childGenes[index] = alternate if newGene == childGenes[index] else newGene
    return ''.join(childGenes)

```


```python
def display(guess):
    timeDiff = datetime.datetime.now() - startTime
    fitness = get_fitness(guess)
    print("{0}\t{1}\t{2}".format(guess, fitness, str(timeDiff)))

random.seed()
startTime = datetime.datetime.now()
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
