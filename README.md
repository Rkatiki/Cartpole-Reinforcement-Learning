# Cartpole-Reinforcement-Learning

Solving the cartpole problem with reinforcement learning

The CartPole environment is a simple environment where the objective is to move a cart left or right in order to balance an upright pole for as long as possible. The state space is described with 4 values representing Cart Position, Cart Velocity, Pole Angle, and Pole Velocity at the Tip. The action space is described with 2 values (0 or 1) allowing the car to either move left or right at each time step.

![alt text](https://thumbs.gfycat.com/GreedyJampackedBlackfish-size_restricted.gif)

### Q-learning
Q-learning learns the action-value function Q(s, a): how good to take an action at a particular state. In Q-learning, we build a memory table Q[s, a] to store Q-values for all possible combinations of s and a. Q-learning is basically about creating the cheat sheet Q. We find out the reward R (if any) and the new state s’. From the memory table, we determine the next action a’ to take which has the maximum Q(s’, a’). 
The Bellman Equation tells us how to update our Q-table after each step we take.

![alt text](https://miro.medium.com/max/2400/0*d3D5g7IxKDHHk8dN)

Basically the agent updates the current perceived value with the estimated optimal future reward which assumes that the agent takes the best current known action. In an implementation, the agent will search through all the actions for a particular state and choose the state-action pair with the highest corresponding Q-value.


### DQN
A DQN, or Deep Q-Network replaces the regular Q-table with a neural network. Rather than mapping a state-action pair to a q-value, a neural network maps input states to (action, Q-value) pairs. The learning process uses 2 neural networks. These networks have the same architecture but different weights. Every N steps, the weights from the main network are copied to the target network. Using both of these networks leads to more stability in the learning process and helps the algorithm to learn more effectively.

![alt text](https://cdn.analyticsvidhya.com/wp-content/uploads/2019/04/Screenshot-2019-04-16-at-5.46.01-PM.png)

### Double DQN
The standard DQN method has been shown to overestimate the true Q-value, because for the target an argmax over estimated Q-values is used. Therefore when some values are overestimated and some underestimated, the overestimated values have a higher probability to be selected.

Standard DQN target:
Q(s<sub>t</sub>, a<sub>t</sub>) = r<sub>t</sub> + Q(s<sub>t+1</sub>, argmax<sub>a</sub>Q(s<sub>t</sub>, a))

By using two uncorralated Q-Networks we can prevent this overestimation. In order to save computation time we do gradient updates only for one of the Q-Networks and periodically update the parameters of the target Q-Network to match the parameter of the Q-Network that is updated.

The Double DQN target then becomes:
Q(s<sub>t</sub>, a<sub>t</sub>) = r<sub>t</sub> + Q<sub>θ</sub>(s<sub>t+1</sub>, argmax<sub>a</sub>Q<sub>target</sub>(s<sub>t</sub>, a))

And the loss function is given by:
(Q(s<sub>t</sub>, a<sub>t</sub>) - Q<sub>θ</sub>(s<sub>t</sub>, a<sub>t</sub>))^2
