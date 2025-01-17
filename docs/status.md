---
layout: default
title:  Status
---
[![175Project](https://res.cloudinary.com/marcomontalbano/image/upload/v1637134840/video_to_markdown/images/youtube--F7zv8Ag7z1w-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=F7zv8Ag7z1w "175Project")
# Project Summary: 
  We implemented a game called CrossTheFireLine, similiar to the Dodge Ball; the agent should learn to avoid getting hit by the fireball from the Ghast as well as  to avoid stepping on the fire block. A ghast that can shoot fireballs will appear in the air and move randomly. When fireball falls to the ground, the stone will be set on fire and these blocks will convert into fire blocks. If the agent steps on the fire or get git by the fireball, it will receive the negative reward. Both our agent Steve and the Ghast's activity space will be limited inside the 60 x 60 area, surrounded by sand walls. Steve needs to observe his surroundings and avoid getting negative reward so that he can survive for longer time. 
  
  The output of the algorithm should be the list of continuous movements.
  
# Approach: 

In order to prevent the agent from walking aimlessly, we confine the agent and ghast to a three-dimensional space. Ghast will continuously shoot fireballs at the agent, and the agent can perform continuous actions to avoid the fireball. Currently, we continued apply the Proximal Policy Optimization (PPO) to train our agent.

We can observe the coming of the Fireball from <ObservationsFromNearbyEntities> the end point of the Fireball can be calculated and a graph is showed by Matplotlib, using the function ax.quiver, with parameters(x,y,z,motionX,motionY, motionZ). 
![image](https://github.com/Chilly712/CrossTheFireLine_Minecraft/blob/main/docs/images/axquiver.png)

## Setup an Environment
<img src="images/env.png" width="650">

## Action
The Ghast can hit several blocks by launching single fireball, so if the agent only take discrete movement, its chances of avoiding the fireball are reduced. Hence, we believe the better choice is to let the agent have continuous movements .

## State
We are using PPO, a policy gradient method for reinforcement learning to train the agent. During the implementation process, we add more input information for the neural network.
Our observation space is a 2x5x5 grid. The first 5x5 grid represents the nearby blocks, containing ['fire', 'stone', 'air'], while the second 5x5 grid shows the possible blocks hitted by the fireballs.

## Reward
It is known that Ghast will launch a fireball at the position where the agent is located, and once the fireball hits the ground, it will ignite the stones on the floor and convert the stones into a fire block. We will get the 5x5 grid around the agent through get_observation() based on agent's location. Since the agent moves randomly during the training process, we use the coordinates of the agent to determine whether it is on the block hit by the fireball. If the agent is in the fire block, a negative reward is given. If the agent steps on the stone successfully, a positive reward will be given.

- Calculation method: 
take the position where ghast is scheduled to launch the fireball as the aim position; take the X position and Y position after the agent takes the action as the current position. If the aim position is included in the 5x5 grid observed by current position, we will update the corresponding block in current grid to fire. Therefore, the agent can repeatedly take the next action based on the latest situation.

By calculating the difference in the health value (Life) returned by each episode of the agent, we can determine whether the agent was hit by a fireball or stepped on the fireblock. If the Life decreases, it means that the agent has not taken a good action, so the negative reward will be given, and vice versa.

The agent’s initial life is 20, and the damage of single fireball is not that huge. Therefore, under certain conditions (for example, all the blocks around the agent are fireblock, and the agent has nowhere to go), the agent is bound to step on the fireblock. While the negative reward is given, we also obtain the survival time of the agent through the timeAlive atttribute in the observation, so we can use the time that the agent survives as the positive reward.


# Evaluation: 
  We want to see a situation where the agent can only make random choices at the beginning, then the agent will die quickly.Howevr, as the number of training times increases, the agent knows what kind of action to preform to keep himself from dying.
  
 1. Survival Time Graph
  
 2. Reward Graph

# Remaining Goals and Challenges:  
  So far, we can observe the Fireball path and avoid getting hit beforehead since it is an entity from <ObservationFromNearbyEntities> within a certain amount of LineSight. We might want to improve how we avoid the fireball. For example, making the agent Steve's vision more wide and open, changing the orientation more often. 
  We can also add the function of fire extinguishing or reflact the fireball back to the ghast, because a fireball can generate a lot of fire blocks, so sometimes it is difficult for the agent to cross the line of fire, if there is a fire extinguishing function will be more convenient for the agent not to receive harm
  we recognize the encounter of the fireball base on the agent's previous location. In the future, we could implement a real-time monitoring on the fireball hitting  the agent utilizing computer vision & Imagenet.
  
  In general, we will change our algorithm in two ways.
  
1. change the input matrix 
  
2. refine the incentives and penalties

# Resources Used:   
  Malmo project documentation and github page to set up the xml environment
  
http://microsoft.github.io/malmo/0.30.0/Schemas/MissionHandlers.html#SchemaPropertiesv
  
https://github.com/microsoft/malmo
  
  PPO.PPOTrainer
  
  Matplotlib for plotting fireball path
  
  NumPy documentation
