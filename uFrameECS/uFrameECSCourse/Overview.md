# uFrame ECS Game Development

## Step 1 - High Level Module Definition
Normally when developing a game we define some high level features that together comprise as the game your developing.  This is exactly what you'll do in uFrame we'll demonstrate by example, but we should define what a module consists of first. Modules define multiple things in uFrame, but the absolute must know are:

- Components - The data structures. Things like Damageable, Targetable, and their properties, health, maxtargets... etc
- Events - These define the information that can be used when communicating between systems.  You could define an event and listen to it all in one module.  Or you can define an event that you publish and other systems can listen for it.
- Systems - These are the behaviours that tie Events and components together, actually performing some kind of rule or logic based on the event that has occurred, and the data that currently resides on our components. After completing specific functionality systems can publish new events to the universe so that any other system that cares about it can handle appropriately.

Ok now lets get back to the example of what this might look like from the view of the game in its entirety.

- SoundFXModule - Listens to all the events where sounds should play and does nothing more than play them.
- VisualFXModule - Listens to all the events where sounds should play and does nothing more than play them.
- CombatModule - Listens for input from the user, signals any kind of AI system, or signals custom events so other modules/systems can do their own things (e.g. GunFired, SwordSlashed)
- EnemiesModule - Handles spawning enemies by category, race, difficulty etc, then publishes events after performing any of those.
- HUDModule - Handles updating the UI based on the current game state. PropertyChanges, collection changes, special events. etc.
- NotificationModule - Similar to HudModule but can be re-used without any coupling to any objects.
- AchievementsModule - A general module for handling and displaying achievements, synchronization to the server, etc..
- MenuModule - Handles the main menu scene, dialogs, menus, buttons, etc
- GameModule - Orchestrates moving too and from game modes, main menus, handles loading multiple scenes together etc.
- PathfindingModule - Listens for events like "NavigateToEnemy", "Stop".. etc
- WavesModule - Basically just a special timer that works randomly base on incrememnts, and providing random race/difficulty levels
- TargetingModule - Handles applying targets, most likely could be as simple as a TargetableComponent, with a Targets collection on it.
- SpellCastingModule - Handles spell casting from a high level, has components specific to spells and their properties.
- UserModule - Handles login/logout.  Publishing user information once a user has logged in.  etc
- ...etc

This list could grow to be fairly large depending on the complexity of the game you're building.  If you get stuck on this part, don't feel like your doing it wrong, clearly defining/naming these guys is arguably one of the most complicated steps.  But remember, it will get your brain ticking, this is a good thing, you want to think first implement second (Ok they both require thinking, but having these clearly defined up front will save you TONS of time and effort moving forward).  
> Note: Even if you have to write it down when pen or paper, always do this step, when writing this I actually figured out more modules that would make sense, and its nothing more than an example.

## The Bridge Abstraction - Keeping things re-usable
> So before explaining this, it is important to note, this is entirely optional.  It will keep your modules very separated and reusable, but does require some extra work up-front.  With that said lets jump into it.

So lets take two modules "EnemiesModule" and "CombatModule" for our explanation and example.  Either one of them could easily be reused in another game, but in your current game when a enemy is spawned it must begin navigating to the player.  This would mean that the EnemyModule must know about the pathfinding system or vice-versa.

 To keep this from happening we can create bridge modules.  Put simply it works like this:
- Modules: The completely seperate pieces of a game.
- Bridge Modules: Modules that are very game specific and connect two or more modules together by listening for one event, processing (if needed) and publishing another event.

Ok, now that you know the idea behind it, here is a step by step example of how this works.
![](../images/BridgeModules.png)
The key thing to note here is the now that we have bridge module, neither the enemy module, nor the pathfinding module know about each other, making them much more re-usable than before.

## Step 2 - Components & Group Definition
Components and Groups are almost the same thing in ECS, they define state,
