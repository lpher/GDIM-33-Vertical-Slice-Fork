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

<img width="1918" height="1198" alt="Screenshot 2026-05-31 222504" src="https://github.com/user-attachments/assets/f385e4e9-a26e-4441-b71a-566b87dba459" />

4. Unity System Used

In this project I used Unity’s NavMesh system on the Enemy so that it can intelligently move and track the player.

## Milestone 3 Devlog
<img width="1917" height="972" alt="Screenshot 2026-06-10 051716" src="https://github.com/user-attachments/assets/0f58026f-16e6-4b6c-a7f1-75dc8e377b75" />


1. My ShaderGraph blends a dry material into a wet one and adds moving water ripple normals and puddle masking to simulate water accumulation on a surface.
   
   For the Fragment Base Color input, I use a UV node and connect its output into a Tiling And Offset node’s UV input. For the Tiling input, I created a Vector2 property named TextureTiling so that I could edit the texture’s tiling dimensions. From there I connected the output of the Tiling And Offset node into a Sample Texture 2D node’s UV input. For the Texture input, I created a Texture2D property named BaseTexture so that I apply different textures for different objects. From there, I multiplied the RGBA output by a color property I named WetColor, then connected the output result to the B input of a Lerp node. For the A input of the Lerp node, I connected the RGBA output from the Sample Texture 2D node and connected a Float property named Wetness to the T input. I then connected the output of the Lerp node into the Fragment node’s Base Color input. This was done to emulate the darkening of color when a surface gets wet. I used this on the walls, floors, player, and enemies as rain was constantly being poured down on them.

   To achieve the moving water ripple normal, I placed another Tiling And Offset node. I connected a UV node to the UV input, a Tiling Vector2 property to the T input, and a Multiply node to the Offset input multiplying a Time node and a RippleSpeed Float property. From there, I connected the output to a Sample Texture 2D node’s UV input and a RippleNormal Texture2D property to the texture input. I then connected the output of the texture node to a Normal Strength node and connected a RippleStrength Float property to the Strength input. From the Normal Strength node, I connected its output to a Lerp node’s B input, a Vector 3 node setting x and y to zero while keeping z at one to the A input, and a RippleOpacity Float property to the T input. Lastly, I connected the Lerp node’s output to the Fragment node’s Normal input. Now the ripple has its own texture that I can edit the tiling of and I can edit how fast it moves, how strong it appears, and how transparent it appears. I mainly use this to simulate a shallow pool at the center of the boss’s arena, but I do use it on the floor and walls to further highlight the details of the textures by setting the speed to 0.

   I also blend a DrySmoothness and WetSmoothness Float property in a Lerp node and connect its output into the Fragment node’s Smoothness input so that I can edit and simulate the reflectiveness of surfaces that comes with them being wet. I use this on every material and object as everything for the most part is being poured on by rain.

2. Based on feedback from testing, I increased the amount of frames the I-frames last for when the player rolls, increased the enemies’ tracking of the player when attacking making their attacks a little more accurate and difficult to avoid, and I increased the recovery time of the enemies’ first attack by lowering the animation speed. The first attack the enemy does has a lot of wind up and deals the most damage in its basic attack chain, so it should leave the enemies more vulnerable if they miss. 

3. Since the last milestone, as it pertains to the main gameplay loop, I’ve added a small dungeon layout with 5 rooms total. The first room is the player’s starting room. It acts as a lobby of sorts. The second through fourth rooms hold lesser enemies who have lower health points and deal less damage. However, as you go further into the dungeon, the number of lesser enemies per room increases. Specifically, it goes from one enemy in the second room to three enemies in the fourth. Once you enter a room, the lesser enemies’ AI will be turned on and a fogwall will be enabled behind you blocking your entrance. Additionally, your path forward is blocked unless you kill at least one lesser enemy in that room. To accommodate the increased number of enemies and maybe the player's lack of souls-like experience, every lesser enemy killed grants the player one extra healing count. Lastly, I added a lock on indicator so that players can better see which enemy they are targeting.

## Milestone 4 Devlog
Milestone 4 Devlog goes here.
## Final Devlog
1. The core gameplay loop consists of moving to progress further in the dungeon, attacking to defeat enemies and build up the mana meter, rolling to dodge enemy attacks, and healing to regain health points on the off chance players get hit by an enemy. The main content you can find include a 5-room dungeon with a 2-phase boss fight and arena at the end, lesser enemies with less health points and damage filling the smaller rooms leading up to the boss arena, a player character with a 3-input attack chain, a dodge roll, a special ability they can use once their mana meter is fully built up, healing capabilities that can increase by killing the lesser enemies, and a togglable lock-on camera that targets the closest enemy. This gameplay and content illustrates to the player what the full game would look like by demonstrating what the player will do at the game’s core. Everything I have chosen to implement is everything that is absolutely necessary for a souls-like game with Jujutsu Kaisen-like abilities. Nothing less and nothing more. Because of this, it effectively communicates the intended gameplay experience and provides players with a clear understanding of what the larger game would be like even with its current smaller scale. Despite being the biggest project I have ever taken up, what I included is only the bare minimum of what I dream of doing one day. For example, both the player and enemy only have 3 basic attacks excluding any special attacks triggered by special conditions. If the game were fully developed to my vision, the amount of basic attacks would increase to 5 for both at the very least to emulate souls-like game standards. By giving players a taste of what is possible with this vertical slice, I hopefully make them ponder what could be possible in the future. This could be more basic attacks like I said earlier, charge attacks, more special abilities, multiple and different enemy types, etc. Something I wanted to implement was a domain expansion, but due to time I could not at this time. Additionally, the game and its content takes inspiration from two well known titles, so people would more likely than not compare the game I’ve made to these existing titles and gauge what the game I made could be based off these titles.

<img width="1917" height="1140" alt="Screenshot 2026-06-11 031554" src="https://github.com/user-attachments/assets/fe51784b-b224-4335-8f88-7e8ac5f32893" />


2. My rendering effect was created with a Fullscreen Shader Graph and it visualizes a colored vignette that pulsates. I made two versions of these, one colored red for blood, and the other colored blue for mana. Both versions are disabled at the start, but activate differently. The blood version activates if the player’s current health points are less than or equal to 50 out of their total 200. If this is not the case, then the effect’s active state is set to false. The mana version activates if the player’s current mana count is equal to the max mana count. The effect’s active state is set to false if this condition is false. These are both activated on the player’s Player Controller graph from its Script Machine component. These overlays adjust the shader's alpha properties to create a pulsing effect. The pulse is generated inside the Shader Graph using Time, Multiply, Sine, Remap, and Lerp nodes to animate the transparency over time. This provides visual feedback to the player that they are in a critical health state for the blood overlay and that their special ability is ready to use for the mana overlay.

3. My process for breaking down a large project into specific systems started with me thinking about the main gameplay loop of the genre I chose to go into. I listed them out to better visualize them. From there I thought about what was necessary and what wasn’t. I asked questions like, “Would the game function with/without this?” What wasn’t necessary I took out of the list. What was left on the list, I then went deeper into those items and broke them down like how I broke down the main gameplay loop. I made more lists and took out what was unnecessary. It’s like a rabbit hole with many levels. It started with the main gameplay loop, which I then broke down into smaller parts, then broke those parts into smaller parts, and I continued until I reached the end. I then thought about how these parts would interact with each other. For example, enemies would interact with the player, dealing damage to them and decreasing their health points. I did this for every part making connections and thinking about how they would interact with other parts. I visualized the connections with a diagram, having boxes be the parts like the enemy or player, and arrows showing what they interacted with and how with concise captions.

      1. If it pertains to using it in the future, then yes, I will use both the bubble diagrams and task break-downs we practiced this quarter in my planning processes because I believe I better understand my game, how it’ll work, flow, and be architected this way. They are basically the manuals to the game I’m architecting. 

      2. Breaking down a large game into smaller steps affects my understanding of the overall scope of the project by getting me to think about the game in a larger scope before even coding, giving me some foresight when developing. I think if I were to jump straight into developing a game, I would be limited in a way that I would be focused on one thing and not have anything else in mind.

      3. What went poorly when I tried executing my plan was that I believed I had enough time at the beginning leading me to taking things lightly. I hadn’t fully realized the full scope of the game I wanted to create. While we had milestones to strive for and planned playtesting sessions, later in the quarter, I started setting personal deadlines of features or content I wanted implemented by, which really helped me progress further in my development. I usually gave myself one day to implement a single feature or content, or two days at the most. What helped was that I was passionate and serious about this project so I kept myself accountable for these personal deadlines I started implementing later in the quarter.

## Open-source assets
- Cite any external assets used here!
