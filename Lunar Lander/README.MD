
#1. Lunar Lander
Description:
This environment is a classic rocket trajectory optimization problem. According to Pontryagin’s maximum principle, it is optimal to fire the engine at full throttle or turn it off. This is the reason why this environment has discrete actions: engine on or off.

There are two environment versions: discrete or continuous. The landing pad is always at coordinates (0,0). The coordinates are the first two numbers in the state vector. Landing outside of the landing pad is possible. Fuel is infinite, so an agent can learn to fly and then land on its first attempt.

To see a heuristic landing, run:

python gymnasium/envs/box2d/lunar_lander.py
Action Space
There are four discrete actions available:

0: do nothing

1: fire left orientation engine

2: fire main engine

3: fire right orientation engine

Observation Space
The state is an 8-dimensional vector: the coordinates of the lander in x & y, its linear velocities in x & y, its angle, its angular velocity, and two booleans that represent whether each leg is in contact with the ground or not.

Rewards
After every step a reward is granted. The total reward of an episode is the sum of the rewards for all the steps within that episode.

For each step, the reward:

is increased/decreased the closer/further the lander is to the landing pad.

is increased/decreased the slower/faster the lander is moving.

is decreased the more the lander is tilted (angle not horizontal).

is increased by 10 points for each leg that is in contact with the ground.

is decreased by 0.03 points each frame a side engine is firing.

is decreased by 0.3 points each frame the main engine is firing.

The episode receive an additional reward of -100 or +100 points for crashing or landing safely respectively.

An episode is considered a solution if it scores at least 200 points.

Starting State
The lander starts at the top center of the viewport with a random initial force applied to its center of mass.

Episode Termination
The episode finishes if:

the lander crashes (the lander body gets in contact with the moon);

the lander gets outside of the viewport (x coordinate is greater than 1);

the lander is not awake. From the Box2D docs, a body which is not awake is a body which doesn’t move and doesn’t collide with any other body:

When Box2D determines that a body (or group of bodies) has come to rest, the body enters a sleep state which has very little CPU overhead. If a body is awake and collides with a sleeping body, then the sleeping body wakes up. Bodies will also wake up if a joint or contact attached to them is destroyed.

Arguments
To use to the continuous environment, you need to specify the continuous=True argument like below:

import gymnasium as gym
env = gym.make(
    "LunarLander-v2",
    continuous: bool = False,
    gravity: float = -10.0,
    enable_wind: bool = False,
    wind_power: float = 15.0,
    turbulence_power: float = 1.5,
)

If continuous=True is passed, continuous actions (corresponding to the throttle of the engines) will be used and the action space will be Box(-1, +1, (2,), dtype=np.float32). The first coordinate of an action determines the throttle of the main engine, while the second coordinate specifies the throttle of the lateral boosters. Given an action np.array([main, lateral]), the main engine will be turned off completely if main < 0 and the throttle scales affinely from 50% to 100% for 0 <= main <= 1 (in particular, the main engine doesn’t work with less than 50% power). Similarly, if -0.5 < lateral < 0.5, the lateral boosters will not fire at all. If lateral < -0.5, the left booster will fire, and if lateral > 0.5, the right booster will fire. Again, the throttle scales affinely from 50% to 100% between -1 and -0.5 (and 0.5 and 1, respectively).

gravity dictates the gravitational constant, this is bounded to be within 0 and -12.

If enable_wind=True is passed, there will be wind effects applied to the lander. The wind is generated using the function tanh(sin(2 k (t+C)) + sin(pi k (t+C))). k is set to 0.01. C is sampled randomly between -9999 and 9999.

wind_power dictates the maximum magnitude of linear wind applied to the craft. The recommended value for wind_power is between 0.0 and 20.0. turbulence_power dictates the maximum magnitude of rotational wind applied to the craft. The recommended value for turbulence_power is between 0.0 and 2.0. 
  
