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

possible development of scope:
- adherence for city limits within the simulation
- humans receive 2 possible attributes - can be infected, not infected, can be dead or alive, recovered or not recovered !
- random number of humans will be infected initially, in case of contact there will be a probability of infection
- infection reduces after recovering (duration of invection 14 timesteps/ages or similar)
- probability of death with infection can be added (human/point stays on map without movement or action)
- plot as two lines (number of people infected / not infected, picture/tendency of development over time and movement of poeple) as termination

