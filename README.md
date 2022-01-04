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

