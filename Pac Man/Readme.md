# Description 
A classic arcade game. Move Pac Man around a maze collecting food and avoiding ghosts- unless you eat a Power Pellet, then you can eat the ghosts too!

## Actions
Pacman has the action space of `Discrete(5)` with the table below listing the meaning of each action’s meanings. 
To enable all 18 possible actions that can be performed on an Atari 2600, specify `full_action_space=True` during initialization or by passing `full_action_space=True` to gymnasium.make.

## Observations
Atari environments have three possible observation types: `"rgb", "grayscale"` and `"ram"`.

1. `obs_type="rgb"` -> observation_space=Box(0, 255, (210, 160, 3), np.uint8)
2. `obs_type="ram"` -> observation_space=Box(0, 255, (128,), np.uint8)
3. `obs_type="grayscale"` -> Box(0, 255, (210, 160), np.uint8), a grayscale version of the “rgb” type
