# Box Optimization with Genetic Algorithm

## Research Questions

## Methods

## Abstract

## Plan of Action
1. Evolutionary vs Genetic Algorithms
2. Class
4. Crossover
5. Mutation
6. Population
7. Individuals
8. Selecting Best Individuals
9. Limitations

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

We want to create all the ```15``` products and we want them in a list. We start by creating an **empty list** in which we will append our products with their attributes:

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

We can then create a **for** loop to print all the products inside our list:

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
We will start with some fundamental definitions:

- The process begins with a set of ```individuals``` which is called a ```Population```. 
- Each **individual** is a ```solution``` to the problem we want to solve.
- An individual is characterized by a set of parameters or variables known as ```Genes```. 
- Genes are joined into a string to form a ```Chromosome (solution)```.
In a genetic algorithm, the set of genes of an individual is represented using a ```string```, in terms of an alphabet. Usually, binary values are used (string of 1s and 0s). 
- We say that we **encode** the genes in a chromosome.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161440105-d3b8e84c-a2c9-4b2d-a3c8-2e127a11921b.png" width="380" height="270"/>
</p>

**Note:** The number ```1``` assigned to a product means that we will select this product to be loaded in our van and the number ```0``` means we are not going to select the product. So our chromosom will be the set of 0s and 1s which indicate which product will be loaded or not.


<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161439741-4bcb15cc-0aeb-4b25-adbc-f924db414ca4.png" width="700" height="120"/>
</p>

In the example above, our chromosome consists of ```15``` genes since we have ```15``` products to choose from. The solutions indicates that the items number **2**, **5**, **6**, **8**, **11**, **13** and **15** which are assigned the number ```1``` should be **selected** whereas the rests which are assgined the number ```0``` should be **discarded**.

Now we will construct a second class called ```Individual``` which will take in a list containing all the **prices** of the products, a list of all the **priorities**, the **space limit** which is ```3``` cubic metres and a variable called **generation** with initial value of ```0```. We will also have an empty list for our **chromosome** which we will initialize with random values.

```
class Individual():
    def __init__(self, spaces, priorities, space_limit, generation=0):  #space_limit = 3
        self.spaces = spaces
        self.priorities = priorities
        self.space_limit = space_limit
        self.generation = generation
        self.chromosome = []
        
        for i in range(len(spaces)):  ## len(spaces) = 15
            if random() < 0.5:
                self.chromosome.append('0')
            else:
                self.chromosome.append('1')
```

We now create our lists which will contain the names, priorities and volumes of the products:

```
names = []
spaces = []
priorities = []

for product in products_list:
    names.append(product.name)
    spaces.append(product.space)
    priorities.append(product.priority)
```

We can now access our lists such that a particular index number of all the ```3``` lists represents the name, volume and priority of that particular item:

```
names[5], spaces[5], priorities[5]
```
```
('Item 6', 0.0035, 4)
```
We will now create our first **individual** object using the Individual Class. It is important to note here that each time we run the cell below, the entries in the chromosome list will change since we are using a random function to initialize it. We will also associate the number ```1``` in our chromosome to the product selected.

```
individual1 = Individual(spaces, priorities)

print('Spaces: ', individual1.spaces)
print('Priorities: ', individual1.priorities)
print('Chromosome: ', individual1.chromosome)

for i in range(len(products_list)):
    if individual1.chromosome[i] == '1':
        print('Name: ', products_list[i].name)
```

```
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353]
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2]
Chromosome:  ['1', '0', '0', '1', '0', '0', '1', '0', '0', '1', '1', '0', '1', '1', '1']
Name:  Item 1
Name:  Item 4
Name:  Item 7
Name:  Item 10
Name:  Item 11
Name:  Item 13
Name:  Item 14
Name:  Item 15
```

### 2. Fitness Function
The fitness function determines how **fit** an individual or chromosome is (the ability of an individual to compete with other individuals). It gives a ```fitness score``` to each individual. The probability that an individual will be selected for reproduction is based on its fitness score.

The question is how are we going to evaluate the individual or chromose as we have a ```maximization``` problem where the goal is to maximize the priority by making use of the maximum capacity of our van. If we had a solution or chromose as shown below then the total volume of the products selected would be ```1.523``` <img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" /> and the total priority would be ```19```. We can already evaluate that this is not a good solution as the capacity of the truck is ```3```<img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" /> and the products selected is about half of that. We will leave Amazon's warehouse with a van half-full which is not efficient at all. A good solution would be one that has the highest total priority value and that do not exceed the space limit.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161442296-82d73636-bc3b-4733-9b78-eb626e20deac.png" width="800" height="200"/>
</p>

We will then create a new function ```fitness``` inside the Individual class. We initialize two variables: **score** and **sum_spaces** both to ```0```. We will update the score variable such that it will contain the total priority value for the items selected and sum_spaces will compute the total volume for the selected items. If our chromosome give us solutions such that sum_spaces > 3 <img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" />, i.e, it will not be possible to load all the slected items in the van, then we will punish the model by assigning it to a low score of ```1```.


```
    def fitness(self):
        score = 0
        sum_spaces = 0
        
        for i in range(len(self.chromosome)):  #len(chromosome) = 15
            if self.chromosome[i] == '1':
                score += self.priorities[i]
                sum_spaces += self.spaces[i]
                
        if sum_spaces > self.space_limit:
            score = 1            # asssign low score if exceed space limit
            
        self.score_evaluation = score
        self.used_space = sum_spaces
```

When we call in our fitness function on individual1, we now get the total score and the used space by the products selected:

```
individual1.fitness()
print('Score: ', individual1.score_evaluation)
print('Used space: ', individual1.used_space)
```

```
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353]
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2]
Chromosome:  ['1', '1', '1', '0', '1', '1', '0', '0', '0', '1', '0', '0', '1', '1', '1']
Name:  Item 1
Name:  Item 2
Name:  Item 3
Name:  Item 5
Name:  Item 6
Name:  Item 10
Name:  Item 13
Name:  Item 14
Name:  Item 15
Score:  27
Used space:  2.9239899000000005
```

Here, we see that we got a total priority value, i.e a score of ```27``` and the total space used, i.e, the total volume of the products selected is ```2.9239899000000005``` <img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" /> which is very good as it is nearly the maximum capacity of our van. Now if we run the code a few times, it will create different solutions because of our random function and we may get the one below:

```
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353]
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2]
Chromosome:  ['1', '0', '1', '0', '0', '1', '1', '0', '1', '0', '1', '1', '0', '1', '1']
Name:  Item 1
Name:  Item 3
Name:  Item 6
Name:  Item 7
Name:  Item 9
Name:  Item 11
Name:  Item 12
Name:  Item 14
Name:  Item 15
Score:  1
Used space:  3.9303999999999997
```
We get a low score of ```1``` because the products selected (```3.9303999999999997``` <img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" />) exceeds the space limit (```3``` <img src="https://latex.codecogs.com/svg.image?\small&space;m^{3}" title="https://latex.codecogs.com/svg.image?\small m^{3}" />) of our van. Therefore, it is impossible to load these products and thus, represents a bad solution.

### 3. Crossover
Crossover combines parts of the chromosome of two ```parents``` to generate more fit ```children```.  For each pair of parents to be ```mated```, a crossover point is chosen at **random** from within the genes. ```Offsprings``` are created by exchanging the genes of parents among themselves until the ```crossover point``` is reached. The population tends to evolve and hence, we can create diversity through this combination.


From the example below we see that Child 1 is composed of the **first part** of Parent 1 before the crossover point and the **second part** of Parent 2 after the crossover point and vice versa for Child 2. We apply this operation because it shows that if we combine the individuals or chromosomes then we will have better results over the generations. Note that Parent 1 and Parent 2 are from the ```first generation``` whereas Child 1 and Child 2 are from the ```second generation``` which is an **evolution** of the first generation.


<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161449949-fd8e4e81-32f2-43cf-ac3e-123fac7efca0.png" width="700" height="150"/>
</p>

In our algorithm we have only one individual so we will need to create a second one to combine the chromosomes to create new individuals. Recall that the ```chromosome``` is the ```solution``` to our problem - the set of 0s and 1s whether a product should be loaded in our van or not. 

```
individual2 = Individual(spaces, priorities, space_limit)

#print('Spaces: ', individual2.spaces)
#print('Priorities: ', individual2.priorities)
#print('Chromosome: ', individual2.chromosome)

for i in range(len(products_list)):
    if individual2.chromosome[i] == '1':
        print('Name: ', products_list[i].name)
        
individual2.fitness()
print('Score: ', individual2.score_evaluation)
print('Used space: ', individual2.used_space)
print('Chromosome: ', individual2.chromosome)
```
Now if we run individual1 we get:
```
Name:  Item 1
Name:  Item 4
Name:  Item 6
Name:  Item 7
Name:  Item 13
Score:  12
Used space:  1.5949
Chromosome:  ['1', '0', '0', '1', '0', '1', '1', '0', '0', '0', '0', '0', '1', '0', '0']
```

And when running individual2 we get:

```
Name:  Item 3
Name:  Item 4
Name:  Item 8
Name:  Item 9
Name:  Item 10
Name:  Item 12
Name:  Item 14
Name:  Item 15
Score:  25
Used space:  2.7773000000000003
Chromosome:  ['0', '0', '1', '1', '0', '0', '0', '1', '1', '1', '0', '1', '0', '1', '1']
```

We see that we have two chromosomes, each one for individual1 and individual2 respectively. We now need to define a new function ```crossover``` to combine the chromosomes. The function has a parameter ```other_individual``` which takes in the chromosome of individual2.

```
    def crossover(self,other_individual):
        cutoff = round(random() * len(self.chromosome)) #to ensure cutoff between 0-15
        print('Cutoff: ', cutoff)
        
        child1 = other_individual.chromosome[0:cutoff] + self.chromosome[cutoff::]
        child2 = self.chromosome[0:cutoff] + other_individual.chromosome[cutoff::]
        
        print(child1)
        print(child2)
```

```
Chromosome:  ['0', '0', '1', '1', '0', '0', '0', '1', '1', '1', '0', '1', '0', '1', '1']

Chromosome:  ['1', '0', '0', '1', '0', '1', '1', '0', '0', '0', '0', '0', '1', '0', '0']

Cutoff:  5
Child 1:  ['0', '0', '1', '1', '0', '1', '1', '0', '0', '0', '0', '0', '1', '0', '0']
Child 2:  ['1', '0', '0', '1', '0', '0', '0', '1', '1', '1', '0', '1', '0', '1', '1']
```

We have a cutoff of ```5``` which means that as from the ```6th``` entry in our parent chromosome we will have a crossover with the second individual. Note that as we run the code again we will have a different cutoff point as the later depends on a random function. 

Now, we need to put both childs in a list called ```children``` because recall that ```child1``` and ```child2``` are also **individuals or chromosomes** so they must also have the attributes ```spaces```, ```priorities```, ```space_limit```, and ```generation``` when we defined our class ```Individual```.

```
        children = [Individual(self.spaces, self.priorities, self.space_limit, self.generation+1),
                   Individual(self.spaces, self.priorities, self.space_limit, self.generation+1)]
        
        children[0].chromosome = child1
        children[1].chromosome = child2
        return children
```
If we now run the code we get **Score** and **Space Used** for **child1**:
```
children[0].fitness()
print('Score: ',children[0].score_evaluation)
print('Used space: ', children[0].used_space)
print('Chromosome: ', children[0].chromosome)
```

```
Score:  16
Used space:  1.2438999999999998
Chromosome:  ['0', '0', '1', '1', '0', '1', '1', '0', '0', '0', '0', '0', '1', '0', '0']
```


**Score** and **Space Used** for **child2**:
```
children[1].fitness()
print('Score: ',children[1].score_evaluation)
print('Used space: ', children[1].used_space)
print('Chromosome: ', children[1].chromosome)
```

```
Score:  1
Used space:  3.1283000000000003
Chromosome:  ['1', '0', '0', '1', '0', '0', '0', '1', '1', '1', '0', '1', '0', '1', '1']
```

Notice that children[0] is the same as child1 and children[1] is the same as child2 except that now we have it as a chromosome and hence, we can compute the space of the products selected and the total priority value.


### 4. Mutation

Mutation is an operation where some individual genes are taken from an offspring and **mutated** or **changed**. This is primarily done to maintain ```diversity``` in the population and also to avoid any ```premature convergence```. Please note here that the mutating locations are chosen at ```random```.  This implies that some of the bits in the bit string can be flipped. 

To sum up: 

- In ```crossover``` we create a **new individual** while in ```mutation``` we create **diversity** in the population by randomly changing the genes of the chromosome.
- ```Mutation``` happens less frequencty compared to ```crossover``` as it happens in nature.
- Mutation changes the genes to a ```low probability``` of approx. ```5%```.
- Mutation of a bit is carried out by changing a ```0``` to ```1``` or vice versa.
-  The mutation of a bit ```does not affect``` the probability of mutation of other bits.


<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161696443-8da10422-6875-4f0c-af8b-7acd7919f0c7.png" width="600" height="100"/>
</p>

We will now define a new function called ```mutation``` have the parameter ```rate``` which indidates the probability of performing mutation. Since we want a lot probability of mutating then we will generate a random number and check if that number is less than our rate. If less then we will change our gene else it remains unchanged. Note that we need to do this process for each gene in our chromosome.

```
    def mutation(self, rate):
        print('Before mutation: ', self.chromosome)
        mutation_number = 0
        for i in range(len(self.chromosome)): #accesing each gene in a chromosome
            if random() < rate: 
                mutation_number += 1
                #print('Mutation occuring...')
                if self.chromosome[i] == '1':
                    self.chromosome[i] = '0'          
                else:
                    self. chromosome[i] = '1'
        print('Number of mutations occurred: ', mutation_number)           
        print('After mutation: ', self.chromosome)
        return self
```
We choose a rate of ```0.05``` which is ```5%``` and we expect to have very little mutation throughout our chromosome:
```
Before mutation:  ['0', '0', '0', '0', '1', '0', '1', '1', '0', '0', '1', '0', '1', '0', '0']
Number of mutations occurred:  2
After mutation:  ['0', '0', '0', '0', '1', '0', '1', '1', '0', '0', '1', '1', '1', '0', '1']
```

As expected we had a low mutation rate -  **twice** at gene number ```12``` and gene number ```15```.

### 5. Population

A population is a set of several ```individuals```. Each individual as its own solution - a set of 0s and 1s. We now need to define the number of individuals that our population will have. We need to perform a test in order to seek the best set of individuals that we need to create. 

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161732540-b79838d9-b7e1-4568-b448-d9f9cc4018d2.png" width="700" height="350"/>
</p>


We will start by creating a new class called ```GeneticAlgorithm``` which will take in the **population size** as parameter. We will have an empty list called ```population``` in which we will append our individuals created by the ```Individual``` class.


```
class GeneticAlgorithm():
    def __init__(self, population_size):
        self.population_size = population_size
        self.population = []
        self.generation = 0
        self.best_solution = None
        self.list_of_solutions = []
        
    def initialize_population(self, spaces, priorities, space_limit):
        for i in range(self.population_size):
            self.population.append(Individual(spaces, priorities, space_limit)) #creating individuals
            
        self.best_solution = self.population[0] #initialize
```
We test with a population size of ```20``` and create our population of ```20``` individuals:
```
population_size = 20
ga = GeneticAlgorithm(population_size)
ga.initialize_population(spaces, priorities, space_limit)

ga.population[5].chromosome
```

We can access our individuals by indexing the ```population``` list and find its chromosome:

```
['1', '0', '1', '0', '0', '0', '0', '0', '1', '0', '0', '1', '0', '1', '1']
```

### 6. Evaluation
Similar to how we evaluated our individual with our ```fitness score``` before, we now need to evaluate our individual ,i.e, evaluate each individual in our population. 

```
for individual in ga.population:
    individual.fitness()
    
for i in range(ga.population_size):
    print('Individual: ', i, '\nSpaces: ', ga.population[i].spaces, '\nPriorities: ', ga.population[i].priorities, 
          '\nChromosome: ', ga.population[i].chromosome, '\nSpace Used: ', ga.population[i].used_space,
          '\nScore: ', ga.population[i].score_evaluation,'\n')
```

```
Individual:  0 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '1', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '1', '1'] 
Space Used:  2.5290899 
Score:  19 

Individual:  1 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '0', '0', '1', '1', '0', '1', '1', '1', '1', '1', '0', '1', '0', '0'] 
Space Used:  3.3707 
Score:  1 

Individual:  2 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '0', '0', '0', '1', '1', '0', '0', '1', '1', '0', '1', '1', '0', '0'] 
Space Used:  2.1737999999999995 
Score:  16 

Individual:  3 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '0', '0', '0', '1', '1', '0', '1', '0', '1', '0', '0', '1', '1', '0'] 
Space Used:  2.2133 
Score:  19 

Individual:  4 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '0', '0', '0', '0', '1', '1', '0', '1', '1', '0', '1', '1', '1', '1'] 
Space Used:  3.3498 
Score:  1 

Individual:  5 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '0', '1', '0', '0', '0', '0', '0', '1', '0', '0', '1', '0', '1', '1'] 
Space Used:  2.5609 
Score:  17 

Individual:  6 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '0', '0', '1', '0', '0', '0', '0', '1', '0', '0', '1', '1', '0', '0'] 
Space Used:  0.8743 
Score:  10 

Individual:  7 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '0', '1', '0', '0', '0', '1', '0', '1', '1', '1', '1', '1', '0', '1'] 
Space Used:  3.3383000000000003 
Score:  1 

Individual:  8 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '1', '0', '1', '0', '0', '1', '0', '1', '0', '0', '0', '1', '0'] 
Space Used:  2.5554899000000004 
Score:  22 

Individual:  9 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '0', '1', '1', '1', '0', '0', '0', '1', '1', '0', '1', '1', '0'] 
Space Used:  3.3309899 
Score:  1 

Individual:  10 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '0', '1', '0', '0', '0', '0', '0', '1', '1', '0', '0', '0', '1', '1'] 
Space Used:  1.9469 
Score:  15 

Individual:  11 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '0', '0', '0', '1', '0', '1', '0', '1', '1', '0', '1', '1', '0', '0'] 
Space Used:  1.9153 
Score:  12 

Individual:  12 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '0', '0', '0', '0', '1', '1', '1', '1', '1', '0', '1', '1', '1'] 
Space Used:  3.7607899 
Score:  1 

Individual:  13 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '0', '1', '1', '0', '1', '0', '0', '0', '1', '0', '0', '1', '1', '1'] 
Space Used:  3.0139000000000005 
Score:  1 

Individual:  14 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '0', '1', '0', '1', '0', '0', '0', '0', '0', '1', '0', '1', '1'] 
Space Used:  2.4225899 
Score:  23 

Individual:  15 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '0', '0', '1', '1', '1', '1', '0', '1', '0', '1', '1', '0', '1'] 
Space Used:  3.0333898999999995 
Score:  1 

Individual:  16 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '0', '0', '0', '1', '0', '1', '0', '0', '0', '0', '0', '0', '1', '1'] 
Space Used:  1.5759999999999998 
Score:  12 

Individual:  17 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '1', '0', '0', '1', '1', '1', '0', '0', '1', '0', '1', '0', '0'] 
Space Used:  2.6173899 
Score:  20 

Individual:  18 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '0', '1', '1', '0', '1', '0', '1', '0', '0', '0', '1', '0', '1', '0'] 
Space Used:  1.7609 
Score:  24 

Individual:  19 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['1', '1', '0', '0', '1', '1', '1', '0', '0', '1', '0', '0', '1', '0', '1'] 
Space Used:  2.4929898999999995 
Score:  18 
```

We calculate the ```score``` of each individual but we do not know which one is the maximum. Hence, we need to sort our individuals in our population to have the ones with the highest score first.

```
    def order_population(self):
        self.population = sorted(self.population, key=lambda population: population.score_evaluation, reverse=True)
```
We now have the score in ascending order with the highest one first:
```
Individual:  0 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '1', '1', '1', '0', '1', '0', '1', '1', '0', '1', '0', '1', '1', '1'] 
Space Used:  2.5722899000000004 
Score:  31 

Individual:  1 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '1', '1', '1', '0', '0', '1', '0', '1', '0', '1', '0', '0', '1', '0'] 
Space Used:  2.6149899000000003 
Score:  23 

Individual:  2 
Spaces:  [0.751, 8.99e-05, 0.4, 0.29, 0.2, 0.0035, 0.496, 0.0424, 0.0319, 0.635, 0.87, 0.498, 0.0544, 0.527, 0.353] 
Priorities:  [1, 3, 5, 5, 4, 4, 1, 2, 1, 2, 3, 3, 1, 5, 2] 
Chromosome:  ['0', '1', '1', '0', '1', '0', '0', '1', '0', '1', '1', '0', '1', '0', '1'] 
Space Used:  2.5548899 
Score:  22 
```
While a list of best solutions is great, we are only interested in the one best solution, i.e, the first element in our population after the ```order_population``` function has been executed.

We will then define a new function ```best_individual``` which will contain the individial from our population with the highest score.

```
    def best_individual(self, individual):
        if individual.score_evaluation > self.best_solution.score_evaluation:
            self.best_solution = individual
```

```
ga.best_individual(ga.population[0])

ga.best_solution.chromosome
```
We see that our best individual has a score of ```31``` and we can also access its chromosome:
```
['0', '1', '1', '1', '0', '1', '0', '1', '1', '0', '1', '0', '1', '1', '1']
```


### 7. Selection
The idea of selection phase is to select the ```fittest individuals``` and let them pass their genes to the ```next generation```. Two pairs of individuals (**parents**) are selected based on their ```fitness scores```. Individuals with high fitness have more chance to be selected for reproduction.


The ```roulette wheel``` selection method is used for selecting all the individuals for the next generation. A roulette wheel is constructed from the relative fitness (**ratio of individual fitness and total fitness**) of each individual.  It is represented in the form of a pie chart where the area occupied by each individual on the roulette wheel is proportional to its relative fitness. Since an individual with better fitness value will occupy a bigger area in the pie chart, the probability of selecting it will also be higher. 

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/161756100-5baf0a1f-06eb-4496-a8ea-7571f148ed18.png" width="460" height="350"/>
</p>

Note however that there is also a low probability of selecting individual with a low score. This is important as it will create more diversity in the algorithm. If we always use the best solution all the time then the population will tend to be composed of similar individuals and lack diversity. 


Now we need to create a function which will simulate this roulette wheel. We first create a function ```sum_evaluations``` which will calculate the **total score** for all the individuals in our population:

```
    def sum_evaluations(self):
        sum = 0
        for individual in self.population:
            sum += individual.score_evaluation
        return sum
```

Next, we will create another function ```select_parent``` which will take in ```sum_evaluation``` as a parameter which is the output of the function ```sum_evaluations```. We will first generate a random number and multiply it by the total score and assign it to a variable called ```random_value```. Then in a while loop we will aggregate each score in the population as from index ```0``` and we select the individual at the index when the aggregate score exceeds the ```random_value```.

```
    def select_parent(self, sum_evaluation):
        parent = -1 #random initialization for variable
        random_value = random() * sum_evaluation
        sum = 0
        i = 0
        print('Random value: ', random_value)
        while i < (len(self.population)) and sum < random_value:
            sum += self.population[i].score_evaluation
            print('i: ', i, ' - ', 'Sum:', sum)
            parent += 1
            i += 1
        return parent
```

For our first parent we select index ```4``` in our population:
```
Random value:  116.30013449371388
i:  0  -  Sum: 32
i:  1  -  Sum: 57
i:  2  -  Sum: 82
i:  3  -  Sum: 107
i:  4  -  Sum: 131
Chromosome:  ['0', '1', '1', '0', '1', '1', '0', '0', '0', '1', '0', '0', '1', '1', '0']
Used Space:  1.8199899
Score:  24
```
For the second parent we select index ```3``` in our population:
```
Random value:  99.877334343137
i:  0  -  Sum: 32
i:  1  -  Sum: 57
i:  2  -  Sum: 82
i:  3  -  Sum: 107
Chromosome:  ['0', '0', '1', '1', '1', '1', '0', '1', '1', '0', '1', '0', '1', '0', '0']
Used Space:  1.8921999999999999
Score:  25
```

Note here that we did not select the best individuals in our population which is of index ```1``` and ```2``` respectively. We chose average ones and this is important as it will help us to create diversity in our population and allow us to achieve better results. However, note that because of our roulette wheel method we have a high probability of selecting the top individuals in our population and a low probability of selecting those with a score of ```1```.

### 8. New Generation
We know how to select ```parents```, create ```children``` using crossover and apply ```mutation``` to those children to have ```diversity```. We will now create a new generation from the functions explained above. 

- Select ```2``` fittest **parents** from our population.
- Create **children** from the parents using crossover.
- Apply **mutation** to children created.
- Add children to **new population**.


We will select ```20``` parents in total to mate so that we will get ```40``` children:
```
new_population = []
mutation_probability = 0.01
for new_individuals in range(0, ga.population_size, 2): # 0, 2, 4,... , 18 
    #print(new_individuals)
    parent1 = ga.select_parent(sum)
    parent2 = ga.select_parent(sum)
    print('Selecting parents: ', parent1, ' - ',parent2)
    print('Parent 1: ', ga.population[parent1].chromosome)
    print('Parent 2: ', ga.population[parent2].chromosome)
    
    children = ga.population[parent1].crossover(ga.population[parent2])
    print('Child 1: ', children[0].chromosome)
    print('Child 2: ',children[1].chromosome)
    
    new_population.append( children[0].mutation(mutation_probability))
    new_population.append( children[1].mutation(mutation_probability))
    print(' ')
```

Below is the first ```3``` results:
```
Selecting parents:  4  -  2
Parent 1:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '1', '0', '0', '0', '0', '1']
Parent 2:  ['0', '0', '0', '1', '1', '1', '0', '0', '1', '0', '1', '1', '1', '1', '1']
Child 1:  ['0', '0', '0', '1', '1', '1', '0', '0', '1', '1', '0', '0', '0', '0', '1']
Child 2:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '0', '1', '1', '1', '1', '1']
Before mutation:  ['0', '0', '0', '1', '1', '1', '0', '0', '1', '1', '0', '0', '0', '0', '1']
After mutation:  ['0', '0', '0', '1', '1', '1', '0', '0', '1', '1', '0', '0', '0', '0', '1']
Before mutation:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '0', '1', '1', '1', '1', '1']
After mutation:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '0', '1', '1', '1', '1', '1']
 
Selecting parents:  1  -  2
Parent 1:  ['0', '1', '1', '1', '0', '0', '0', '1', '1', '1', '0', '1', '1', '1', '1']
Parent 2:  ['0', '0', '0', '1', '1', '1', '0', '0', '1', '0', '1', '1', '1', '1', '1']
Child 1:  ['0', '0', '0', '1', '1', '1', '0', '1', '1', '1', '0', '1', '1', '1', '1']
Child 2:  ['0', '1', '1', '1', '0', '0', '0', '0', '1', '0', '1', '1', '1', '1', '1']
Before mutation:  ['0', '0', '0', '1', '1', '1', '0', '1', '1', '1', '0', '1', '1', '1', '1']
After mutation:  ['0', '0', '0', '1', '1', '1', '0', '1', '1', '1', '0', '0', '1', '1', '1']
Before mutation:  ['0', '1', '1', '1', '0', '0', '0', '0', '1', '0', '1', '1', '1', '1', '1']
After mutation:  ['0', '1', '1', '1', '0', '0', '0', '0', '1', '0', '1', '1', '1', '1', '1']
 
Selecting parents:  4  -  5
Parent 1:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '1', '0', '0', '0', '0', '1']
Parent 2:  ['0', '0', '1', '1', '1', '0', '0', '0', '1', '0', '0', '0', '1', '1', '0']
Child 1:  ['0', '0', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '0', '0', '1']
Child 2:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '0', '0', '0', '1', '1', '0']
Before mutation:  ['0', '0', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '0', '0', '1']
After mutation:  ['0', '0', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '0', '0', '1']
Before mutation:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '0', '0', '0', '1', '1', '0']
After mutation:  ['1', '1', '1', '0', '1', '1', '0', '0', '1', '0', '0', '0', '1', '1', '0']
```

Note that the code above is creating only ```one``` new generation. However, we need to create several new generations using a ```stopping criterion```. We will first create a function which will allow us to visualize the best individual from each generation.

```
    def visualize_generation(self):
        best = self.population[0]
        print('Generation: ', ga.population[0].generation,
              'Chromosome: ', best.chromosome,
             'Total priorities: ', best.score_evaluation,
             'Total Space Used: ', best.used_space)
```



### 9. Complete Genetic Algorithm
Finally, we already have bits and pieces of our genetic algorithm. We just need to put them in place as in our process flow. We create a function called ```solve``` which will take in the ```mutation_probability```, ```number_of_generations``` which is our **stopping criterion**, ```spaces```, ```prices``` and ```space limit``` as parameters.

1. Create **initial population** of individuals.
2. Evaluate initial population using **fitness score**.
3. **Order** individuals to put the fittest one at the top of the population.
4. Set **number of generations**  - stopping criterion.
5. Select **fittest parents** from each generation.
6. Create **children** using **crossover**.
7. Apply **mutation** to new children.
8. Append children to **new generation**.
9. Discard **old generation** to append **new generation** in population list.
10. Evaluate **current population** using fitness score.
11. Select **best individual** in each generation.
12. Select **best generation**.

```
    def solve(self, mutation_probability, number_of_generations, spaces, prices, limit):
        
        # create initial population
        self.initialize_population(spaces, prices, limit)
        
        # calculate fitness score for each individual
        for individual in self.population:
            individual.fitness()
            
        # order individuals to put fittest first
        self.order_population()
        
        # visualize fittest individual
        self.visualize_generation()
        
        # Stopping criterion - number of generations to create
        for generation in range(number_of_generations):
            sum = self.sum_evaluations()
            new_population = []
            for new_individuals in range(0, ga.population_size, 2): # 0, 2, 4,... , 18 
                
                # select parents
                parent1 = self.select_parent(sum)
                parent2 = self.select_parent(sum)
                
                # create new children
                children = self.population[parent1].crossover(ga.population[parent2])
                
                # apply mutation and append children to new population
                new_population.append( children[0].mutation(mutation_probability))
                new_population.append( children[1].mutation(mutation_probability))
             
            # discard old population
            self.population = list(new_population)
           
            #evaluate current population
            for individual in self.population:
                individual.fitness()
                
            # visualize best individual in each generation
            self.visualize_generation()
            best = self.population[0]
            self.best_individual(best)
        
        print('\n','*** Best Solution ***', '\n',
            'Generation: ', self.best_solution.generation, '\n',
            'Chromosome: ', self.best_solution.chromosome, '\n',
            'Total priorities: ', self.best_solution.score_evaluation, '\n',
            'Total Space Used: ', self.best_solution.used_space)
    
        return self.best_solution.chromosome
```

We will create a population of ```20``` individuals for ```100``` generations with the following parameters:

```
limit = 3
population_size = 20
mutation_probability = 0.01
number_of_generations = 100 
```

Here's the best individuals from the first 11 generations:

```
Generation:  0  -  Total priorities:  30  -  Total Space Used:  2.9074899  -  Chromosome:  ['0', '1', '1', '1', '1', '1', '1', '0', '1', '1', '0', '1', '0', '0', '1']
Generation:  1  -  Total priorities:  18  -  Total Space Used:  2.1383899  -  Chromosome:  ['1', '1', '0', '1', '0', '0', '1', '1', '1', '0', '0', '0', '0', '1', '0']
Generation:  2  -  Total priorities:  17  -  Total Space Used:  1.6423899  -  Chromosome:  ['1', '1', '0', '1', '0', '0', '0', '1', '1', '0', '0', '0', '0', '1', '0']
Generation:  3  -  Total priorities:  16  -  Total Space Used:  1.7403000000000002  -  Chromosome:  ['0', '0', '0', '1', '0', '0', '1', '1', '1', '0', '0', '0', '0', '1', '1']
Generation:  4  -  Total priorities:  29  -  Total Space Used:  2.8638898999999998  -  Chromosome:  ['1', '1', '1', '1', '1', '1', '0', '0', '1', '1', '0', '1', '1', '0', '0']
Generation:  5  -  Total priorities:  26  -  Total Space Used:  2.5947899  -  Chromosome:  ['1', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '1', '1', '1', '0']
Generation:  6  -  Total priorities:  21  -  Total Space Used:  2.7007898999999997  -  Chromosome:  ['1', '1', '1', '1', '0', '0', '1', '1', '1', '1', '0', '0', '1', '0', '0']
Generation:  7  -  Total priorities:  22  -  Total Space Used:  2.3663898999999997  -  Chromosome:  ['1', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '1', '0', '0', '1']
Generation:  8  -  Total priorities:  24  -  Total Space Used:  1.6437898999999998  -  Chromosome:  ['0', '1', '0', '1', '1', '0', '0', '1', '1', '0', '0', '1', '1', '1', '0']
Generation:  9  -  Total priorities:  22  -  Total Space Used:  1.3457899000000002  -  Chromosome:  ['0', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '0', '1', '1', '0']
Generation:  10  -  Total priorities:  26  -  Total Space Used:  2.5947899  -  Chromosome:  ['1', '1', '1', '1', '0', '0', '0', '1', '1', '0', '0', '1', '1', '1', '0']
```

Our optimal solution was found at Generation ```84``` with a total space used of ```2.9928898999999998```<img src="https://latex.codecogs.com/svg.image?m^{3}" title="https://latex.codecogs.com/svg.image?m^{3}" />, a total priority value of ```35``` and a total percentage volume occupied in our van of ```99.762 %``` which is extremely efficient:

```
 *** Best Solution *** 
 Generation:  84 
 Chromosome:  ['0', '1', '1', '1', '1', '1', '0', '0', '1', '1', '0', '1', '1', '1', '1'] 
 Total priorities:  35 
 Total Space Used:  2.9928898999999998
 Percentage of volume used:  99.762 %
```

We can also get the names of the items which we will load in our van:

```
Name:  Item 2  - Priority:  3  - Volume:  8.99e-05
Name:  Item 3  - Priority:  5  - Volume:  0.4
Name:  Item 4  - Priority:  5  - Volume:  0.29
Name:  Item 5  - Priority:  4  - Volume:  0.2
Name:  Item 6  - Priority:  4  - Volume:  0.0035
Name:  Item 9  - Priority:  1  - Volume:  0.0319
Name:  Item 10  - Priority:  2  - Volume:  0.635
Name:  Item 12  - Priority:  3  - Volume:  0.498
Name:  Item 13  - Priority:  1  - Volume:  0.0544
Name:  Item 14  - Priority:  5  - Volume:  0.527
Name:  Item 15  - Priority:  2  - Volume:  0.353
```

<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/162053138-63b57649-3209-4b3e-840b-a714b76075d7.png" width="850" height="170"/>
</p>


### 10. Visualization
Visualization will help us better in analyzing the best individual in each generation. We already have an empty list called ```list_of_solutions``` so we will append our best individual for each generation in it and create a graph using ```Plotly```:

Every time we run the code we will get a new solution so for this trial we got our maximum score of ```34``` at generation ```46```. Notice that our graph has 2 troughs at generation ```41``` and ```83```. This is because the solution exceeded the maximum capacity of the truck and so the solution had a low score of only  ```1```.

![newplot (3)](https://user-images.githubusercontent.com/59663734/161955614-27094015-c147-418d-add2-ea349bb0e484.png)

The graph below shows the total space used for all the generations. Our solution at generation ```46``` has a volume of ```2.91529``` <img src="https://latex.codecogs.com/svg.image?m^{3}" title="https://latex.codecogs.com/svg.image?m^{3}" />. The red dashed line shows the maximum capacity of the truck. We see that the solutions exceeded the limit at two instances:  generation ```41``` and ```83```.

![newplot (4)](https://user-images.githubusercontent.com/59663734/161955636-86bdfdd2-2284-4801-9ada-f0a74ed94bcd.png)


When we plot both data in one graph we see that the troughs of the total score coincide the peaks of the total space used. This is because for solutions which has a total used space > ```3``` <img src="https://latex.codecogs.com/svg.image?m^{3}" title="https://latex.codecogs.com/svg.image?m^{3}" /> we gave them a score of ```1```.
![newplot (6)](https://user-images.githubusercontent.com/59663734/161955950-c2b2bfb4-ab2f-48d8-9191-6598e5463f37.png)


Note that at generation ```65``` we had a higher total space used of ```2.94579``` <img src="https://latex.codecogs.com/svg.image?m^{3}" title="https://latex.codecogs.com/svg.image?m^{3}" /> however at this generation we had a total score of only ```25```. So although we had a solution which gives us a much higher total space used, we could not optimized for the total score.

### 11. Limitations

While our Genetic Algorithm works perfectly fine in selecting the products which both optimize the ```total volume occupied``` and the ```total priority value```, one parameter that it does not take into account is the ```dimensions``` of the boxes. And this is a crucial parameter as although the algorithm shows us that a specifc box of a sepecific volume should be loaded in the truck then we have to be aware that it is not always feasible to load the box if the dimensions are not taken into account. 


<p align="center">
  <img src= "https://user-images.githubusercontent.com/59663734/162056324-1bb9a520-5469-4e9f-9c10-965793494aa1.png" width="700" height="290"/>
</p>

In the example above we see that both boxes have a volume of ```8``` <img src="https://latex.codecogs.com/svg.image?m^{3}" title="https://latex.codecogs.com/svg.image?m^{3}" />. However, the box of dimensions ```2m x 2m x 2m``` can easily be loaded in the van which the one with dimensions ```1m x 1m x 8m``` is protruded from the van. Our algorithm defined above could tell us to choose a box of volume ```8``` <img src="https://latex.codecogs.com/svg.image?m^{3}" title="https://latex.codecogs.com/svg.image?m^{3}" /> for instance but when taking the dimensions into account we see that it is not feasible.












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
