# SCI-TECH Projects
## Reinforcement Learning:
-This can be a subset of ML, where mainly there's an agent who does some action on environment and the env returns the reward. The agent learns his next action from the reward and acts accordingly to increase it. 

### **Cart-Pole Env**: Here the agent moves the cart in a fixed space and the goal is to keep the pole straight.

**Modules used**:
1. os - Operating System - basic module to work with system
2. stable_baselines3 - module for working with env and contains all the required algorithms for RL
3. gym/Gymnasium - we preferred gym here while Gymnasium is just an updated version of the same - This is used to generate the env.
   
**Creating Env** :
Here, we already have the env in the module, we just load it, using gym.make()

About the env:

Action space : This is all that the agent can perform, in this case it is of Discrete[2] which means only two values,


0 - to move the cart left

1 - to move the cart right

Observation space : This is a Box[ ] of 4 parameters which are, Cart distance, Cart velocity, Pole angle and Pole angular velocity.

**Training Model** :
We vectorize our env using DummyVecEnv function and then use the Mlp policy, verbose is set to 1 to improve our model with necessarhy hyperparameters. Since we don't have Pytorch, this uses cpu instead of cuda. We train our model for 20000 steps here which can be adjusted. All this is super cool since it learns all of it by itself!!! Kudos to the module!

Now it is important for us to know when an ep actually ends...so first,if pole angle is greater then 12 degrees, or cart position reaches the end, that is greater than 2.4 on either sides and if the length of ep passes 500.

**Reward** : Since the goal is to keep the pole upright for as long as possible, a reward of +1 for every step taken, including the termination step, is allotted. The threshold for rewards is 475 for v1.

Alright, so next is **Evaluation**!

Even for this we imported evaluate_policy from sb3, which takes in our model, env and number of episodes to evaluate as arguments. What it does is, first, resets env before every ep, then starts predict funciton and examines the reward. It returns two values, Average reward and Standard deviation.
Then its our time to actually test our model!

So what we do is, first set a fixed number of episodes, then we define a couple of variables to store our values, remember that env.reset() returns the observations from env so this is fed to agent for next action.

Done - to check if ep is done

Score - to collect the reward

Action - predicting action from obs

Step function is an inbuilt funciton, returns all the required parameters after the action.

**That's it**! Now we refine our model by adjusting the learning steps and can go deeper by adjusting the other hyperparameters like learning rate, discount factor and batch size.

Here we do an additional step by adding callback,

This is like we define the frequency to evaluate and what it does is to check if our reward reached a certain thershold value and stops if it true. The threshold here is set to 200 and frequency as 10000.

### **Reaction Wheel Pendulum** : Here we have an inverted pendulum, the action is to move the wheel and our goal is to keep the pendulum straight.

Modules used :

Otherthan the previous modules, we have numpy - as we know, it has many uses

**Creating Env** : We need to create our env here, so we start with defining our constraints,

-gravity, length, mass, inertia of wheel and pendulum, max pendulum angle, max angular velocity of pendulum and reaction wheel, dt is the time step we follow here.

1. reset - this just sets the state to some random position
2. step - First, takes in the action, angular acceleration and updates theta(position of pendulum), omega(angular velocity of pen) and omega_rw(ang. vel. of rw). Then calculates the torque and resultant angular acceleration of pendulum. From there we go for angular velocity and position.  We calculate the reward considering both position and velocity and we add 1 just to make it positive.

An episode is done if angle is greater than max angle or velocity is greater then max angular velocity.

This returns the final state, reward, done and then an empty dictionary for any additional information

**Checking Model** :

Then we train, evaluate and test our model in a very similar way as before but using 50000 steps here.

**That's it!** We then refine our model by iteration!
