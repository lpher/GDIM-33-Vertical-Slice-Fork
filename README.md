# GDIM33 Vertical Slice
## Milestone 1 Devlog
[Vertical Slice Repository with my commits.](https://github.com/lpher/GDIM-33-Vertical-Slice)

1. In my game, I have a Player Controller Graph which contains a node chain that calculates camera-relative movement direction for the player and stores it each frame. The node chain does this by getting the camera’s forward and right vectors from a transform cameraTransform variable. From those vectors, the graph then extracts the X and Z-values from each of them and creates a new vector 3 where Y equals zero. This removes vertical tilt from the camera direction and keeps movement grounded. After that the graph gets the player’s input through a vector 2 moveInput variable. From the variable, it extracts the X and Y-values to scale forward and right direction for W/S and A/D inputs respectively through multiplication with the flattened vectors the graph made before. The results are then added together to form a single movement vector, which is then normalized to prevent diagonal movement from moving faster. On a fixed update node, the normalized result is saved into a vector 3 moveDirection variable, and then ran through an if node that checks whether or not its magnitude is greater than 0.1. If false, do nothing, but if true, set the vector 3 lastMovementDirection variable to the value of the moveDirection variable. Additionally, through this, the graph creates movement relative to the camera and maintains direction even when inputs stop.

2. [Updated break-down.](https://docs.google.com/presentation/d/1fOkdzATHGMJyJxChTzLwguQFGH2ylNccHMmCnEcj208/edit?usp=sharing)
   
   In my break-down, I added the Player State Machine logic in the Player bubble. Under the State Machine, I added the individual states I currently have implemented, that being the Player’s idle, movement, and rolling states. After that, I drew additional arrows and added comments to show which systems the State Machine interacted with and how it interacted with those systems. Lastly, I highlighted my changes in yellow to have my changes be more identifiable.

   The Player State Machine I implemented for milestone one controls the transitions between the idling, moving, and rolling states. It allows only one of these major player actions to be active at a single time. In the Move State, the Physics System handles the Player’s normal movement. But when transitioning to the Roll State, normal movement is paused and only the rolling movement runs until the roll itself is actually finished. This State Machine interacts with the Physics System for movement, as when it transitions into the Move State, the state runs a node chain using the Physics System to make the Player move. The State Machine interacts with the Animation System by using the animations for idling, moving, and rolling and triggering them when receiving certain inputs to transition between them.

## Milestone 2 Devlog
1. Feature Task Break-down
      Summary
         The Black Flash allows the player to dish out a significant amount of damage in one hit, rewarding the player for surviving long enough, evening the odds against the enemy, and briefly putting the player in an enhanced state.
      2-3 Big Steps From Least to Most Complex
         1. Creating a dedicated state that can be transitioned too and from any existing state
            3-5 Substeps to Take Towards Completion
               1. Download animation
               2. Place new animation in animator controller and apply transitions in and out of the state
               3. Create new animator variable/parameter in animator window and assign it to the transitions
               4. Trigger the animation in the state machine when entering the new state
               5. Playtest to see if the animation plays when entering the state
         2. Adding and assigning an animation that can be transitioned too and from any existing animation
         3. Setting up the hitbox window, visual and camera effects, damage values, and linking it to an enhanced state for the player

## Milestone 3 Devlog
Milestone 3 Devlog goes here.
## Milestone 4 Devlog
Milestone 4 Devlog goes here.
## Final Devlog
Final Devlog goes here.
## Open-source assets
- Cite any external assets used here!
