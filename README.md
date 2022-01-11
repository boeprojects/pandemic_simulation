# pandemic_simulation

Simple OOP Study on pandemic infection progression

Based on SIR Modell:

•	Deteministic, continuous model (vs descrete model with finite population)
•	Constant term beta => how many others are infected by virus positive person per time unit (per day) => 
  S'(t)=-beta*S(t)*I(t)
  
•	Out of I(t) infected persons (%-tage of population) emerge beta*I(t) infected persons after a day (e.g. 15% if beta=3/2)
– in this case everybody is possibly to be infected – more real only S(t) is infectable
(e.g. 90% still to be infected as 10% are already infected in this example) and therefore beta will be multiplied by S(t)

•	Every day beta*S(t)*I(t) infected persons are added
•	The number of infected persons grows every day by beta*S(t)*I(t) (as percentage, all variables are scaled between 0 and 1)
  I'(t)=beta*S(t)*I(t) – alpha*I(t)

•	Per time unit (everey day) a part of infected persons alpha passes away / recovers [no differenciation]. Hence 1/alpha is defined as average duration of desease (example 7 days)
  R'(t)=alpha*I(t)

Base reproduction number R0 = beta as infections per day and 1/alpha as duration of desease in days = beta/alpha (e.g. number of infections caused by infected person during whole desease period)
The model consists of three variables:
    S (susceptibles)
    I (infected)
    R (recovered/removed)
The value of the variables moves between 0 and 1, interpreted as 0% to 100% of population.

- Basic conception class man, city and methods walk and update
```python
class Mensch:
    def __init__(self, x, y):
        self.x, self.y = x, y
    
    def gehen(self, dx, dy): # Delta x und y als Veränderung
         self.x, self.y = self.x+dx, self.y+dy

class Stadt:
    def __init__(self, n):
        self.menschen = [Mensch(0, 0) for i in range(n)]#wir arbeiten mit einer list comprehension
        
    
    def update(self): # neue Stadtmethode als Zeitschritt
        for mensch in self.menschen:
            mensch.gehen(3, 6)# da keine Variable dx/dy existent hier, muss ich Werte eingeben!

            # Aufgabe: Mensch zufällig bewegen, in Werten zwischen -1 und 1 auch als floats!
            # Dann die Methode Update von außen aufrufen !
        
karlsruhe = Stadt(10)
bob = Mensch(2, 5)
```
- Menschen bewegen sich im KO-System

![virus_001](https://user-images.githubusercontent.com/67191365/148960764-8ac5a1e7-7788-4030-b4de-12b464221bb2.PNG)

- Wir definieren delta x und delta y als Veränderung
- Menschen bewegen sich zufällig in der Methode update
```python
import numpy as np


class Mensch:
    def __init__(self, x, y):
        self.x, self.y = x, y
    
    def gehen(self, dx, dy): # Delta x und y als Veränderung
         self.x, self.y = self.x+dx, self.y+dy

class Stadt:
    def __init__(self, n):
        self.menschen = [Mensch(np.random.uniform(-100, 100), np.random.uniform(-100, 100)) for i in range(n)]#wir arbeiten mit einer list comprehension
        self.history = []
        self.age = 0
        self.save()
            
    def update(self, n=1): # neue Stadtmethode als Zeitschritt
        for _ in range(n): # wäre i, da es nicht genutzt wird nehmen wir Unterstrich (soll n mal genutzt werden)
            for mensch in self.menschen:
                mensch.gehen(np.random.uniform(-1, 1), np.random.uniform(-1, 1))# da keine Variable dx/dy existent hier, muss ich Werte eingeben!
            self.age += 1
            self.save()
    
    def save(self):
        for mensch in self.menschen:
            self.history.append([self.age, mensch.x, mensch.y])# muss eine liste in liste sein
                
    def pprint(self):
        for mensch in self.menschen:
            print(mensch.x, mensch.y) # wir greifen auf die Laufvariable x und y über die Variable mensch zu! mensch enthält ein Objekt vom Typ Mensch!
        
        
karlsruhe = Stadt(100)
karlsruhe.update()
karlsruhe.update(n=1000)
len(karlsruhe.history) # history nur attribut ohne liste , keine () o.ä.
```
result 100200

### Visualiierung Pandas (Werte) und Plotly (Video)
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

mögliche Erweiterungen:
- Grenze der Stadt einhalten
- Menschen sollen 2 mögliche Attribute erhalten, kann infiziert oder nicht oder kann tot sein oder nicht, genesen oder nicht !
- Zufällige Zahl der Menschen werden initial infiziert sein, wenn sie aufeinander treffen wird es eine Wahrscheinlichkeit der Ansteckung geben
- Ansteckung verringert sich nach Genesung (Dauer der Infektion 14 timesetps/ages o.ä.)
- Wahrscheinlichkeit des Todes bei Infektion kommt hinzu, Mensch bleibt auf Karte ohne Bewegung und Aktion
- Plot als 2 Linien (Anzahl Leute infiziert / nicht infiziert, Sehen/Tendenz wie es sich über Zeit verringert und die Bewegungen der Menschen) als Abschluss

