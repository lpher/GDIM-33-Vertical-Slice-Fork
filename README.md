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

               1. Create new action on player input system and assign a key to activate it
               2. Create boolean variable and have pressing the assigned key set it to true
               3. Create new state and transitions to it
               4. In these transitions, check if boolean variable is true, then transition to new state
               5. Set up a debug log when entering the new state, then playtest and check the console to ensure the state was successfully transitioned too

         2. Adding and assigning an animation that can be transitioned too and from any existing animation

            3-5 Substeps to Take Towards Completion

               1. Download animation
               2. Place new animation in animator controller and apply transitions in and out of the state
               3. Create new animator variable/parameter in animator window and assign it to the transitions
               4. Trigger the animation in the state machine when entering the new state
               5. Playtest to see if the animation plays when entering the state

         3. Setting up the hitbox window, visual and camera effects, damage values, and linking it to an enhanced state for the player

            3-5 Substeps to Take Towards Completion

               1. Create an empty game object and add a collider to it to act as the attack hitbox, setting the collider to “is trigger.” Disable this game object
               2. Set up some trigger to enable and disable the game object to create the attack window
               3. Learn or download particle system effects, editing them as needed, and making them prefabs
               4. In an “On Trigger Enter” method, instantiate these prefabs. Additionally, create a set a new boolean variable to true to check if you landed a Black Flash
               5. Playtest to check if the hitboxes are working and activate the visual effects. From there you can add damage values, have the new boolean variable being set to true be the transition out of the state, and have the other states check if the player has landed a Black Flash and change their behavior accordingly

2. Task Break-down Aid

This task break-down helped me in building this feature by allowing me to first brainstorm and visualize the steps I needed to take. Breaking this feature down into actionable steps also helped me think about what systems it would use and interact with. The task break-down essentially gave me a plan or instruction manual to follow so that I could tackle architecting with some level of direction and preparedness. Something I could do that I did not initially to improve my break-downs and level of preparedness is to research beforehand on how to specifically do things I’m not entirely sure of like watching youtube videos. I was researching how I would architect some systems as I was already developing, which may have made the development process take longer.

3. Bridging Visual Scripting and C# Coding

I bridged visual scripting and C# coding through having the graph controlling when the Black Flash lands call a public C# method named “PlayBlackFlashImpact()” from a “BlackFlashFeedback.cs” script. Doing this separates the logic between landing the Black Flash and playing the visual effects of the Black Flash making my graph less cluttered and more simplified.

[InsertImageHere]

4. Unity System Used

In this project I used Unity’s NavMesh system on the Enemy so that it can intelligently move and track the player.

## Milestone 3 Devlog
Milestone 3 Devlog goes here.
## Milestone 4 Devlog
Milestone 4 Devlog goes here.
## Final Devlog
Final Devlog goes here.
## Open-source assets
- Cite any external assets used here!
