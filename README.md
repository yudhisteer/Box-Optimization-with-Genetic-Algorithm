# Box Optimization with Genetic Algorithm


## Abstract

## Research Questions

## Problem Statement
Genetic algorithm can be hard to understand if one does not have a Biology background hence, for the purpose of this project we will initially assume we are a lucky delivery guy for Amazon Prime whose job will be deliver the products people ordered. However, it is us who has to decide which parcels to take in the van as we have a limited volume capacity. 

**P.S.** You do not want to be like this guy below. But instead you want to make the choice of selecting each box in a clear scientific way that will maximize two things:

- the ```total volume``` of all the parcels which will be transported
- the ```priority value``` associated with each parcel

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/160805259-ff84e180-0830-41d4-8cf5-d29e2f821fde.jpeg" width = "600" height = "350"/>
</p>

So before we get started we need to declare some variables for our problem. 

- Max van capacity: ```10``` <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" />
- Each product are packaged in a box and each box has a ```priority value``` and ```volume``` associated with it.
- The volume of the box is calculated by: ```length x width x height``` and the unit will be <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" />. Initially we will only have the volume given to us instead of the dimensions. In the second part of the project we will study this limitation and improve our model.
- The priority of a product is on a range of ```1 to 5``` with ```1``` being the **lowest** priority and ```5``` being the **highest** priority.
- A product can have a **large volume** - a refrigerator for example - 0.6 <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" /> - but a **low priority** of ```1```.
- On the other hand, a product such as a smartphone can have a **low volume** of 0.0000525 <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" /> but a **high priority** of ```5``` to be delievered. 
- Naturally, we will have a ```maximization``` problem whereby we want to maximize the number of parcels (volume) to be carried in our van and maximize all the high priority parcels first.

To add more content, we will visually display our problem statement below which will allow us to get a bigger picture of the dilemma we are facing.










## Plan of Action
1. Evolutionary vs Genetic Algorithms
2. Fitness Function
3. Crossover
4. Mutation
5. Population
6. Individuals
7. Selecting Best Individuals
8. Limitations

### 1. Evolutionary vs Genetic Algorithms

### 2. Fitness Function

### 3. Crossover

### 4. Mutation

### 5. Population

### 6. Individuals

### 7. Selecting Best Individuals

### 8. Limitations

## Conclusion

## References
1. https://towardsdatascience.com/introduction-to-genetic-algorithms-including-example-code-e396e98d8bf3
2. https://stackoverflow.com/questions/2890061/what-is-the-difference-between-genetic-and-evolutionary-algorithms
3. https://www.mathworks.com/help/gads/what-is-the-genetic-algorithm.html
4. https://medium.com/geekculture/introduction-to-genetic-algorithm-d417119040b7
