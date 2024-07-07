A swinging pendulum (when we simplify it down) has two variables defining it: the angle at which it is hanging, θθ, and the speed at which it is moving, ω:
![[../../../assets/Coding/Simple_gravity_pendulum.png|300]]

Once we have these equations we can either:

1.  Do some maths and solve some equations to calculate a general solution to the problem (e.g.) $$\theta (t) = A\cos(\omega t - \phi)$$ (this process usually involves "integration"), or
2.  use the equations to work out what the forces are on the weight at the beginning, use that to work out what it will be in a 100th of a second, then jump forward by a 100th of a second and do that again. Repeating this until we've found our answer.

## Numerical simulation

A general equation for a numerical simulation is:
$$state_{t} = rules(state_{t-1})$$

In the case of an exponential decay the state value is single number, v and each time step the rule is that the value should change by a factor, λ, also called the "decay rate".
$$rules:v\mapsto(\lambda v)$$
and:
$$v_{t} = rules(v_{t-1})$$

assume the decay rate is 0.9 (λ = 0.9) and start the simulation with a value (v0) of 100:
$$v_{1} = rules(v_{0})=\lambda v_{0} = 0.9\times 100 = 90$$
then:
$$v_{2} = rules(v_{1})=\lambda v_{1} = 0.9\times 90 = 81$$

and go on...

## Automating the algorithm
Using Python code:
```Python
INITIAL_VALUE = 100
DECAY_RATE = 0.9

NUMBER_OF_STEPS = 5

v = INITIAL_VALUE

for t in range(NUMBER_OF_STEPS):
    v *= DECAY_RATE
    print(v)
---
90.0
81.0
72.9
65.61000000000001
59.049000000000014
```

As module:
```Python  
def simulation(INITIAL_VALUE, DECAY_RATE, NUMBER_OF_STEPS): 
	import numpy as np 
    value_history = np.zeros(NUMBER_OF_STEPS)  
    value_history[0] = INITIAL_VALUE  
  
    for t in range(1, NUMBER_OF_STEPS):  
        value_history[t] = value_history[t-1] * DECAY_RATE  
    print(value_history)  
  
  
simulation(100, 0.9, 20)
---
[100.     90.     81.     72.9    65.61   59.049]
```

save as file:
```Python
def simulation(INITIAL_VALUE, DECAY_RATE, NUMBER_OF_STEPS):  
    value_history = np.zeros(NUMBER_OF_STEPS)  
    value_history[0] = INITIAL_VALUE  
  
    for t in range(1, NUMBER_OF_STEPS):  
        value_history[t] = value_history[t-1] * DECAY_RATE   
    numpy.savetxt("decay", value_history, fmt="%.5f")  # %.5f小数点5位; .%d整数  
  
  
simulation(100, 0.9, 20)
---
decay.txt
100.00000  
90.00000  
81.00000  
72.90000  
65.61000  
59.04900  
53.14410  
47.82969  
43.04672  
38.74205  
34.86784  
31.38106  
28.24295  
25.41866  
22.87679  
20.58911  
18.53020  
16.67718  
15.00946  
13.50852
```

plot:
```python
sns.set_theme()  
  
with np.load("decay.npz") as f:  
    value_history = f["state"]  
  
g = sns.relplot(value_history, kind="line").set(  
    xlim=(0,None),  
    ylim=(0,None),  
    xlabel="Time step",  
    ylabel="v"  
)  
plt.show()
```
![[../../../assets/Coding/Stimulation_pendulum.png|350]]