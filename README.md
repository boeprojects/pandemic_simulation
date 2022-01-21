# pandemic_simulation

Simple OOP Study on pandemic infection progression

Based on SIR Modell:

•	Deteministic, continuous model (vs descrete model with finite population)
•	Constant term beta => how many others are infected by virus positive person per time unit (per day) 

S'(t)=-beta*S(t)*I(t)
  
•	Out of I(t) infected persons (%-tage of population) emerge beta*I(t) infected persons after a day (e.g. 15% if beta=3/2)
– in this case everybody is possibly to be infected – more real only S(t) is infectable
(e.g. 90% still to be infected as 10% are already infected in this example) and therefore beta will be multiplied by S(t)

•	Every day beta*S(t)*I(t) infected persons are added
•	The number of infected persons grows every day by beta*S(t)*I(t) (as percentage, all variables are scaled between 0 and 1)

  I'(t)=beta*S(t)*I(t) – alpha*I(t)

•	Per time unit (everey day) a part of infected persons alpha passes away / recovers [no differenciation]. Hence 1/alpha is defined as average duration of desease (example 7 days)

R'(t)=alpha*I(t)

Basic Reproduction Number R0 = beta as infections per day and 1/alpha as duration of desease in days = beta/alpha (e.g. number of infections caused by infected person during whole desease period)

The model consists of three variables:

    - S (susceptibles)
    - I (infected)
    - R (recovered/removed)
The value of the variables moves between 0 and 1, interpreted as 0% to 100% of population.

- Dynamic Programming: these Code snippets show a stepwise development of features within the oop modeling process
- Basic conception class "human", "city" and methods "walk" and "update"
```python
class Human:
    def __init__(self, x, y):
        self.x, self.y = x, y
    
    def walk(self, dx, dy): #delta x and y as change/modification
         self.x, self.y = self.x+dx, self.y+dy

class City:
    def __init__(self, n):
        self.humans = [human(0, 0) for i in range(n)]#working with list comprehension
        
    
    def update(self): #new method of city as step in time
        for human in self.humans:
            mensch.walk(3, 6) #at this point, variables dx/dy are not existing - therefore it is important to input values!

        
karlsruhe = City(10)
bob = Human(2, 5)
```
- humans are mooving inside the coordinate system

![virus_004](https://user-images.githubusercontent.com/67191365/149169551-2ab02c27-cedc-450d-9d66-8d32242a4a50.PNG)


- we define delta x and delta y as modification/change
- humans are mooving randomly within the method "update"
```python
import numpy as np


class Human:
    def __init__(self, x, y):
        self.x, self.y = x, y
    
    def walk(self, dx, dy): #delta x and y as change/modification
         self.x, self.y = self.x+dx, self.y+dy

class City:
    def __init__(self, n):
        self.humans = [Human(np.random.uniform(-100, 100), np.random.uniform(-100, 100)) for i in range(n)]#working with list comprehension
        self.history = []
        self.age = 0
        self.save()
            
    def update(self, n=1): #new method of city as step in time
        for _ in range(n):  # normally, i is used, in case we don't use this variable, we take underscore
            for human in self.humans:
                mensch.walk(np.random.uniform(-1, 1), np.random.uniform(-1, 1))#at this point, variables dx/dy are not existing - it is important to input values!
            self.age += 1
            self.save()
    
    def save(self):
        for human in self.humans:
            self.history.append([self.age, human.x, human.y]) #must be a list in list!
                
    def pprint(self):
        for human in self.humans:
            print(human.x, human.y) #accessing indexed/running variable x and y over the variable "human". (human contains an object of type human!).
        
        
karlsruhe = City(100)
karlsruhe.update()
karlsruhe.update(n=1000)
len(karlsruhe.history) #history is at this point only an attribute without list, no () or similar!
```
result 100200

### Visualization Pandas (values) and Plotly (video)
```python
import pandas as pd

df = pd.DataFrame(hannover.history, columns=['age', 'x', 'y'])
df
```

![virus_002](https://user-images.githubusercontent.com/67191365/148966091-67f30303-9e45-4c5e-ab72-d866e5a0fa0a.PNG)

```python
import plotly.express as px

px.scatter(df, x='x', y='y', animation_frame='age', range_x=[-100, 100], range_y=[-100, 100], width=1000, height=1000)
```

![virus_003](https://user-images.githubusercontent.com/67191365/148966466-158f2487-5fce-46bd-993d-99a6951d6b29.PNG)

## Possible development of scope:
•	adherence for city limits within the simulation – human stay into borders of City (-100, 100) instead of randomizes leaps (method walk loop (self.x = 100 if self.x > 100 else self.x etc.)

•	human receives 2 possible attributes - can be infected, not infected, recovered or not recovered

•	random number of humans will be infected initially

•	in case of contact there will be a probability of infection

•	infection reduces after recovering (duration of infection 14 timesteps/ages or similar)

•	probability of death with infection can be added (human/point stays on map without movement or action)

•	plot as two lines (number of people infected / not infected, picture/tendency of development over time and movement of people) as termination

```python
class Human:
    def __init__(self, x, y, uid, status='S'):#unique identifier uid
        self.x, self.y = x, y
        self.uid = uid
        self.status = status
        self.infected_time = 0 #attribute default zero
        
    def update(self, max_infected_time=14):
        self.walk(np.random.uniform(-1, 1), np.random.uniform(-1, 1))#values for dx/dy [as variables that eists in method walk]!
        if self.status == 'I':
            self.infected_time += 1
            if self.infected_time > max_infected_time:
                self.status = 'R' #"status" approaches  14 days infection time and gets new value R=Recovered - infection reduces after recovering in 14 timesteps

    
    def walk(self, dx, dy): #delta x and delta y as change/modification of x, y
        self.x, self.y = self.x+dx, self.y+dy # x or y can be bigger or smaller than (-)100 in this position !
        self.x = 100 if self.x > 100 else self.x #adherence for city limits within the simulation
        self.x = -100 if self.x < -100 else self.x
        self.y = 100 if self.y > 100 else self.y
        self.y = -100 if self.y < -100 else self.y
        

class City:
    def __init__(self, n, days, params):
        self.days = days
        self.params = params
        self.humans = [Human(np.random.uniform(-100, 100), np.random.uniform(-100, 100), i, random.choice(['S', 'I', 'R'])) for i in range(n)]
        self.dummies = [Human(-1002, 1000, -1), Mensch(-1002, 990, -2, status='I'), Human(-1002, 980, -3, status='R')]
        # defining self.dummies in order to compensate random effects [.e.g. stepwise augmentation of status without leaps]
        self.history = []
        self.age = 0
        self.save()
        self.update(n=self.days)
            
    def update(self, n=1): # new method of City as timestep
        for _ in tqdm(range(n)): # #normally, i is used, in case we don't use this variable, we take underscore (should be uses n-times)
            for human in self.humans:
                human.update(max_infected_time=self.params[2])
                if human.status == 'S': # check with status 'S'
                    for fellow in self.humans: #must change indexed/running variable [fellow]
                        if human.uid != fellow.uid:
                            distance = math.sqrt(((fellow.x-human.x)**2)+((fellow.y-human.y)**2)) 
                            #euklidean distance, calculation of distance between human and fellow human [scalar product]
                            if distance < self.params[0] and fellow.status == 'I': 
                            #after this, in case of contact there will be a probability of infection, therefore random
                                r = random.random() 
                                    human.status = 'I'
                                    break
                            
            self.age += 1 #age as timesteps continues to max! (max_infected_time) over update method!
            self.save()#saving history of timesteps

             
    def save(self):# representing historical coordinates of timesteps of object "human" in a list of values
        for human in self.humans:
            self.history.append([self.age, human.x, human.y, human.uid, human.status])#attention to datatype list within list
        for human in self.dummies:
            self.history.append([self.age, human.x, human.y, human.uid, human.status])
    
    def show(self):
        df = pd.DataFrame(self.history, columns=['age', 'x', 'y', 'uid', 'status'])
        return px.scatter(df, x='x', y='y', color='status', color_discrete_map={'I': 'red', 'R': 'green', 'S': 'blue'}, animation_frame='age', animation_group='uid', range_x=[-105, 105], range_y=[-105, 105], width=1000, height=1000)
                
    def pprint(self):
        for human in self.menschen:
            print(human.x, human.y) #accessing indexed/running variable x, y over variable human that contains an object of type human
```
### Different Settings of parameters to simulate a simple picture of infection progress as follows
```python
s = City(100, 1000, (1, 0.3, 14))# Number of humans of city (100), 1000days, plus parameter (distance 0.3 for infection of fellow human)
s.show()
```
![virus_005](https://user-images.githubusercontent.com/67191365/150544281-784dc9c2-39c5-4f59-9723-8993ce5f5329.PNG)
![virus_006](https://user-images.githubusercontent.com/67191365/150544406-edcb5f87-fc89-4ebe-8639-6c80e700c83e.PNG)
![virus_007](https://user-images.githubusercontent.com/67191365/150544459-30a456f0-1d4c-4c57-b5bb-f1421992b120.PNG)
![virus_008](https://user-images.githubusercontent.com/67191365/150544501-09b97d06-3cff-4c37-87f8-d4d8dfc788b8.PNG)
![virus_009](https://user-images.githubusercontent.com/67191365/150544541-8fbc0ea6-92f3-4782-af89-e47865a0722c.PNG)



