# Representation-of-a-Real-World-Problem-as-a-Markov-Decision-Process

## Register number : 212224040352
## Developed by : Thavanesh B


## Aim

To identify a real-world sequential decision-making problem, an Automated Warehouse Robot, and represent it as a Markov Decision Process (MDP) by defining its states, actions, transition probabilities, rewards, and implementing the MDP using Python.

---

## Problem Statement
### Problem Description

To develop an RL-based robot that can navigate a warehouse, pick up and move objects, scan inventory, and optimize supply chain operations. The robot continuously observes its surroundings (position, obstacles, battery, stock levels) and decides what to do next. The objective is to maximize warehouse efficiency, keep stock available, avoid collisions and battery failures, and complete delivery tasks with minimum delay.



## MDP Components

$$MDP = (S, A, P, R, \gamma)$$

<img width="872" height="350" alt="image" src="https://github.com/user-attachments/assets/3597bdba-2a89-4df1-acf6-e1a271e5fb46" />




## State Space

The full state description of the robot contains:

Current position and direction of the robot
Location of objects and obstacles
Warehouse inventory levels
Robot battery level
Warehouse layout
Stock availability and supply chain status

```
S = {
    Idle at Docking Station,
    Object Located,
    Carrying Object,
    Low Inventory Detected,
    Low Battery
}
```





## Sample State

Current State = Object Located



---

## Action Space

```
A = {
    Move to Object,            (move forward / backward / left / right)
    Pick Up Object,
    Place Object,
    Scan Inventory,
    Replenish Stock,
    Select Supplier and Route,
    Recharge Battery
}

```


## Sample Action

Action = Pick Up Object

## Transition Probability

$$P(s' \mid s,a)$$

Probability of reaching next state $s'$ after taking action $a$ in current state $s$.

Transitions are stochastic because of obstacles, grip failures, battery drain and supplier delays. For every $(s,a)$ pair the probabilities sum to 1.

<img width="832" height="851" alt="image" src="https://github.com/user-attachments/assets/605a31ba-c37b-4ad2-8f59-ac194cf93aec" />



## Reward Function

$$R(s,a,s')$$

<img width="838" height="562" alt="image" src="https://github.com/user-attachments/assets/250e910c-4cc9-472e-b56b-27d6b2dca357" />



## Graphical Representation

Write your answer here.

Draw the MDP graph.

The graph should include:

1. States as nodes.
2. Actions as arrows.
3. Rewards on transitions.
4. Transition probabilities if applicable.


---

## Python Representation
```
print("Name: THAVANESH B")
print("Register Number: 212224040352\n")

states = ["Idle at Docking Station","Object Located","Carrying Object","Low Inventory Detected","Low Battery"]
actions = ["Move to Object","Pick Up Object","Place Object","Scan Inventory","Replenish Stock","Select Supplier and Route","Recharge Battery"]

I,O,C,L,B = states
# (state, action) -> [(next_state, probability, reward)]
T = {
 (I,"Move to Object"):[(O,0.85,2),(I,0.10,-1),(B,0.05,-2)],
 (I,"Scan Inventory"):[(L,0.30,5),(I,0.70,1)],
 (O,"Pick Up Object"):[(C,0.90,5),(O,0.10,-2)],
 (C,"Place Object"):[(I,0.90,15),(C,0.05,-3),(B,0.05,10)],
 (L,"Replenish Stock"):[(I,0.70,8),(L,0.30,-2)],
 (L,"Select Supplier and Route"):[(I,0.90,12),(L,0.10,-3)],
 (B,"Recharge Battery"):[(I,0.95,-1),(B,0.05,-1)],
}
gamma = 0.9

print("States:"); [print("-",s) for s in states]
print("\nActions:"); [print("-",a) for a in actions]
print("\nTransitions P(s'|s,a) and R(s,a,s'):")
for (s,a),outs in T.items():
    assert abs(sum(p for _,p,_ in outs)-1)<1e-9
    for s2,p,r in outs: print(f"({s}, {a}) -> {s2} | P={p} | R={r}")
print("\nDiscount Factor:",gamma)

V = {s:0 for s in states}
for _ in range(1000):
    V = {s:max(sum(p*(r+gamma*V[s2]) for s2,p,r in outs) for (ss,a),outs in T.items() if ss==s) for s in states}
print("\nOptimal State Values:")
for s in states: print(f"V({s}) = {V[s]:.2f}")
print("\nOptimal Policy:")
for s in states:
    best=max(((a,sum(p*(r+gamma*V[s2]) for s2,p,r in outs)) for (ss,a),outs in T.items() if ss==s),key=lambda x:x[1])
    print(f"{s} -> {best[0]}")
```

## Output

<img width="872" height="757" alt="image" src="https://github.com/user-attachments/assets/0dde5f05-5dd3-41ef-b72c-ee69b41a9bd4" />


## Result

The Automated Warehouse Robot was successfully represented as a Markov Decision Process (MDP) with defined states, actions, transition probabilities, rewards and a discount factor of 0.9. Value iteration on the model gives a sensible optimal policy: move to the object, pick it up, place it, restock using supplier and route selection when inventory is low, and recharge when the battery is low.



---

