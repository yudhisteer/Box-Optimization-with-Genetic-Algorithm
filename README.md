# Box Optimization with Genetic Algorithm


## Abstract

## Research Questions

## Problem Statement
Genetic algorithm can be hard to understand if one does not have a Biology background hence, for the purpose of this project we will initially assume we are a lucky delivery guy for Amazon Prime whose job will be deliver the products people ordered. However, it is us who has to decide which parcels to take in the van as we have a **limited volume capacity**. 

**P.S.** You do not want to be like this guy below. But instead you want to make the choice of selecting each box in a clear scientific way that will maximize two things:

- the ```total volume``` of all the parcels which will be transported
- the ```priority value``` associated with each parcel

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/160805259-ff84e180-0830-41d4-8cf5-d29e2f821fde.jpeg" width = "600" height = "350"/>
</p>

So before we get started we need to declare some variables for our problem. 

- Max van capacity: ```3``` <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" />
- Each product are packaged in a box and each box has a ```priority value``` and ```volume``` associated with it.
- The volume of the box is calculated by: ```length x width x height``` and the unit will be <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" />. Initially we will only have the volume given to us instead of the dimensions. In the second part of the project we will study this limitation and improve our model.
- The priority of a product is on a range of ```1 to 5``` with ```1``` being the **lowest** priority and ```5``` being the **highest** priority.
- A product can have a **large volume** - a refrigerator for example - 0.6 <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" /> - but a **low priority** of ```1```.
- On the other hand, a product such as a smartphone can have a **low volume** of 0.0000525 <img src="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" title="https://latex.codecogs.com/png.image?\dpi{110}m^{3}" /> but a **high priority** of ```5``` to be delievered. 
- Naturally, we will have a ```maximization``` problem whereby we want to maximize the number of parcels (volume) to be carried in our van and maximize all the high priority parcels first.

**Note:** The ```priority``` parameter is optional in the sense like we could have ```price``` instead or the amount of ```attachment``` we have for an object. We just need a second parameter which we want ```maximize``` - that is the higher the value, the better profitable it will be.

To add more content, we will visually display our problem statement below which will allow us to get a bigger picture of the dilemma we are facing.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/160875541-0ea1c7f4-036d-4ee7-8792-0e784de3fe8e.png" width="600" height="270"/>
</p>

As a trial, we will have ```15``` items in total. Not all of them will fit in the van so our model will need to find the best combination of items:

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/160901405-4dccf911-8db9-4775-9685-77d73890ac48.png" />
</p>

The total volume of all the items is ```5.143``` cubic metres and the maximum capacity of our van is only ```3``` cubic metres. Therefore, <img src="https://latex.codecogs.com/png.image?\dpi{110}\geq&space;" title="https://latex.codecogs.com/png.image?\dpi{110}\geq " /> ```2.143``` cubic metres of product will need to be discarded when selecting the best possible solution. Now that we have setup our problem statement, we are ready to tackle the subject.


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


#### 1.1 Theory of Evolution

Evolution is the process by which modern organisms have descended from ancient organisms over time. In his book ```"On the Origin of Species"``` published in 1859, ```Charles Darwin``` states that all species of organisms arise and develop through the **natural selection** of small, inherited variations that increase the individual's ability to **compete**, **survive**, and **reproduce**. 


<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161418405-b01b9558-33f2-4736-bfd9-1ef09fcc1280.jpg" width="480" height="200" />
</p>

_“It is not the strongest of the species that survives, nor the most intelligent, but the one most responsive to change.”_ - Charles Darwin

Charles Darwin popularized the concept of ```survival of the fittest``` as a mechanism underlying the natural selection that drives the evolution of life. Organisms with genes better suited to the environment are selected for survival and pass them to the next generation.

#### 1.1 Natural Selection

Sometimes it is not about having the most strength, good looks, intelligence or any such favourable features. Quite often unexpected things happen. In many environments, the individuals most likely to survive are not the strongest or the best looking, but those with many ```adaptive features```. They can more easily develop new skills if their living conditions suddenly change. Sometimes, it is even pure luck that determines if you are selected or not to survive or die. Below are detailed explanations about the process from Khan Academy.

Darwin's theory was based on the mechanism of natural selection, which explains how populations can evolve in such a way that they become **better suited to their environments** over time.

_According to the theory, individuals with traits that enable them to adapt to their environments will help them survive and have more offspring, which will inherit those traits. Individuals with less adaptive traits will less frequently survive to pass them on. Over time, the traits that enable species to survive and reproduce will become more frequent in the population and the population will change, or evolve. Through **natural selection**, Darwin suggested, genetically diverse species could arise from a **common ancestor**._



<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161418796-cda4a3ab-d414-40f7-975a-b6ece1095421.png" width="800" height="300"/>
</p>


Individuals have **variations** within their heritable traits. Some variations make an individual better suited to survive and reproduce in their environment. For example, Darwin recognized that finches with beaks adapted to the specific food sources present on an island were more likely to survive and pass their genes to the next generation. Birds with the right beaks were defined as the fittest.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161419215-c33ea2d0-73da-4989-b118-7f2a900cbab2.png" width="480" height="450"/>
</p>

If these variations continues over generations, these favorable **adaptations** (the heritable features that aid survival and reproduction) will become more and more common in the population. The population will not only evolve (change in its genetic makeup and inherited traits), but will evolve in such a way that it becomes adapted, or better-suited, to its environment.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161419319-e61b6b10-dcf6-4021-8ff7-74ae8e263628.png" width="700" height="400"/>
</p>

To sum up:
- generation after generation, natural selection acts as a kind of sieve, or a remover of undesirable traits. 
- Organisms therefore gradually become better-suited for their environment. 
- If the environment changes, natural selection will then push organisms to evolve in a different direction to adapt to their new circumstances.

#### 1.2 Artificial Selection
Artificial selection is the identification by humans of desirable traits in plants and animals, and the steps taken to enhance and perpetuate those traits in future generations. Artificial selection works the same way as natural selection, except that with natural selection it is ```nature```, not ```human``` interference, that makes these decisions.

```Dog breeding``` is a prime example of artificial selection. Although all dogs are descendants of the wolf, the use of artificial selection has allowed humans to drastically alter the appearance of dogs. For centuries, dogs have been bred for various desired characteristics, leading to the creation of a wide range of dogs, from the tiny Chihuahua to the massive Great Dane.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161425895-3c30389f-b7b9-46ac-81c4-f4f36420bef7.jpg" width="700" height="300"/>
</p>
             
             
Today, bull terriers are bred to have a football-shaped head and thick, squat body – a far cry from the lean and handsome dog of 1915. Somewhere along its journey to a mutated skull and thick abdomen the bull terrier also picked up a number of other maladies like supernumerary teeth and compulsive tail-chasing.

Many fruits and vegetables have been improved or even created through artificial selection. For example, broccoli, cauliflower, and cabbage were all derived from the ```wild mustard plant``` through selective breeding. Artificial selection appeals to humans since it is ```faster``` than natural selection and allows humans to ```mold``` organisms to their **needs**.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161419867-3d66dfba-e244-4ef5-8432-ff1dd23bdb70.png"/>
</p>


#### 1.3 Evolutionary Algorithm
An ```Evolutionary Algorithm (EA)``` uses mechanisms inspired by biological evolution, such as **reproduction**, **mutation**, **recombination**, and **selection**. Evolutionary Algorithms are widely used as a practical, robust optimization and search methods. There are numerous variants of evolutionary algorithms: ```genetic algorithms (GA)```, ```evolutionary strategies```, ```evolutionary programming```,  and  ```genetic programming```. Hence, Genetic Algorithm is a **subset** of Evolutionary Algorithm that simulates or models Genetics and Evolution (biological behavior) to optimize a highly complex function.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161417026-c28df20f-6b93-40cd-a346-ef9dd522cd33.png" />
</p>

#### 1.4 Genetic Algorithm
The genetic algorithm (GA) is a method for solving both constrained and unconstrained optimization problems that is based on ```natural selection```, the process explained above that drives biological **evolution**. GA was developed by ```John H. Holland``` and his students and colleagues at the University of Michigan, of which David E. Goldberg is the most notable.


So how does it work? The genetic algorithm repeatedly modifies a population of individual solutions. At each step, the genetic algorithm selects individuals from the current population to be parents and uses them to produce the children for the next generation. Over successive generations, the population ```"evolves"``` toward an **optimal solution**. 

The genetic algorithm uses ```3``` main types of rules at each step to create the next generation from the current population:

- ```Selection``` rules select the individuals, called **parents**, that contribute to the population at the next generation. The selection is generally **stochastic**, and can depend on the individuals' scores.

- ```Crossover``` rules **combine** two parents to form **children** for the next generation.

- ```Mutation``` rules apply **random** changes to individual parents to form children.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161427650-3b3a7cce-8e2e-4347-ac6e-560f20382f84.png" width="550" height="700"/>
</p>

### 2. Class
We will now build ```2``` class: one for the ```product``` and second for each ```individual``` item. We start with the product class:

#### 2.1 Product Class

All of the boxes have ```3``` attributes: **name**, **price** and **volume**. We will then build a class called ```Product``` which will allow us to generate items which will have these mentioned attributes. 

```
class Product():
    def __init__(self, name, space, priority):
        self.name = name
        self.space = space
        self.priority = priority
```

We create our first item **named** ```Item 1``` which has a **volume** of ```0.751``` <img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" /> and **priority** ```1```. We assign it to a variable called ```p1```:

```
p1 = Product('Item 1', 0.751, 1)

p1.name, p1.space, p1.priority
```

```
('Item 1', 0.751, 1)
```

We want to create all the ```15``` products and we want them in a list. We start by creating an empty list in which we will append our products with their attributes:

```
products_list = []
products_list.append(Product('Item 1', 0.751, 1))
products_list.append(Product('Item 2', 0.0000899, 3))
products_list.append(Product('Item 3', 0.400, 5))
products_list.append(Product('Item 4', 0.290, 5))
products_list.append(Product('Item 5', 0.200, 4))
products_list.append(Product('Item 6', 0.00350, 4))
products_list.append(Product('Item 7', 0.496, 1))
products_list.append(Product('Item 8', 0.0424, 2))
products_list.append(Product('Item 9', 0.0319, 1))
products_list.append(Product('Item 10', 0.635, 2))
products_list.append(Product('Item 11', 0.870, 3))
products_list.append(Product('Item 12', 0.498, 3))
products_list.append(Product('Item 13', 0.0544, 1))
products_list.append(Product('Item 14', 0.527, 5))
products_list.append(Product('Item 15', 0.353, 2))
```

We can then create a for loop to print all the products inside our list:

```
for product in products_list:
    print(product.name, ' - ', product.space, ' - ', product.priority)
```

```
Item 1  -  0.751  -  1
Item 2  -  8.99e-05  -  3
Item 3  -  0.4  -  5
Item 4  -  0.29  -  5
Item 5  -  0.2  -  4
Item 6  -  0.0035  -  4
Item 7  -  0.496  -  1
Item 8  -  0.0424  -  2
Item 9  -  0.0319  -  1
Item 10  -  0.635  -  2
Item 11  -  0.87  -  3
Item 12  -  0.498  -  3
Item 13  -  0.0544  -  1
Item 14  -  0.527  -  5
Item 15  -  0.353  -  2
```


#### 2.2 Individual Class






















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
5. https://www.livescience.com/474-controversy-evolution-works.html
6. https://theconversation.com/what-does-survival-of-the-fittest-mean-in-the-coronavirus-pandemic-look-to-the-immune-system-137355
7. https://polarpedia.eu/en/natural-selection-and-adaptation/
8. https://www.nationalgeographic.org/encyclopedia/artificial-selection/#:~:text=Artificial%20selection%20is%20the%20identification,those%20traits%20in%20future%20generations.
