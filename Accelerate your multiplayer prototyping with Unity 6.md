## Presented by: Esteban E. Maldonado Caban - Sr. Developer Advocate at Unity

#game-design  #progamming 

---

### Fundamental Problem with Multiplayer Game Dev
* Multiplayer games allow people to have a shared experience
	* online multiplayer removes the requirement of physically being together and using the same device
		* The issue is this is the problem of data communication: 
			* making sure the game states (the states of each player, the state of other elements in the game, etc) for each player are reliably communicated between the two or more devices that those players are using

### Unity 6 Bits
* Multiplayer Center 
	* New tool allows you to import packages for implementing multiplayer based on the kind of experience you would like to create
		* Choosing between genres and player count
			* These decisions aren't hard set and can be changed later on
			* Gives recommendations for hosting 
		* Seems to work like the package manager in this regard
	* Netcode and Tools
		* section in this tool that can create scenes that have the needed network components to get started
			* Temp UI GO - default UI to show how it works, can and probably should be deleted
			* Network Manager GO
			* As well as the basic starter elements 
		* New Multiplayer Play Mode under Window > Multiplayer
			* Can generate virtual players so that the multiplayer functionality can be tested without having to build out the project to do so
				* Creates what's essentially an instance of the game on a virtual machine that hosted locally
			* Why have this?
				* Unity got recommendations (probably requests) to implement a network/netcode solution into the editor/engine
* Actually creating something online
	* Creating the needed UI that have the needed functions attached for managing the lobby/session (checking player count, joining and leaving, getting info about it, etc)
		* Create new Gameobjects > multiplayer widgets 
			* > create > ...
			* > join and leave > ...
			* > join and leave > ...
			* > info > session player list
			* > info > session code
	* !! Need to have the "Connect to Unity Cloud" check checked in order for this work 
		* Don't when creating the project 
		* This can be checked under the Services tab in the build settings
	* If everything is set up right, it'll automatically connect to the service and authenticate 
* Network manager
	* in charge of starting session and letting others join
		* Holds the needed info to allow for those functions 
	* Has a reference (or references) to the prefabs of the players that will be instantiated when a player joins
		* Network prefabs list
			* contains a list of the need prefabs, looked like a scriptable obj of some sort
			* The demo had the player object, the bullet object, and I think also the enemy game obj (demo was a 3rd person shooter)
			* Network GO
				* scripts don't inherit from Monobehavior, it inherits from a NetworkBehavior class
					* Needs/should have code that checks to make sure you're only updating from the right point of view of authority
						* If you keep this in mind, you can write code as if its single player game.
	* Client Network Transform
		* A script that grabs the needed transform info of each player in order to communicate it with the other device(s)
		* Is on the gameobjects in the game that need to have their data communicated
	* There are also network animator components and a network rigidbody component
* Remote procedure calls
	* Can be used to alert only certain ones and edits certain ones or etc
		* Doesn't reference the exact client obj, it references the instance of that client that exists in the server/host 
	* These can also be used for communicating things like character select between clients/players
		* For example, if each character can only be played my one player at a time. If one player choses one of say 4 characters first, that character is unavailable to other players that are in the current session.
* There are some tools to use for debugging 
	* Scene window
		* Checking the bandwidth 
		* Checking what device/client owns what
	* Network Simulator 
		* Can simulate different network conditions based off certain profiles 
	* The profiler also has some things
		* NGO objects tab

### Misc 
* ECS/DOTS not compatible with this as it has it's own netcode solution
	* Is more code heavy and requires more technical know how
* The Multiplayer center netcode solution is indeed rather limited in its functionality and not as customizable
	* Works with gameobjects and not the DOTS/ECS system
	* Not as fast as the DOTS netcode solution
* There are ways to mitigate latency issues
	* fixed update for physics 
	* a custom late update for sending cosmetic changes that isn't essential data/info
* Transferring session ownership
	* Can chose distributed authority in the multiplayer center when importing packages
		* transfer authority over of the session to another client when the owner drops
		* is good for some thing that has a lot of objects but less than 10 players
			* since all of them own the session or at least bits of the session
			* ownership of the session is essentially divided across the clients 
* Git__ - youtube tutorial for a small cart racing game that uses DOTS?
* Project center 
	* future feature intended to make implementing other live services easier

### Unity Group Stuff
* Unity as a company wants to encourage people in local gamedev/unity communities to form groups 
	* Unity would be willing to sponser these and would send staff from the company to talk 


 *GO = Game Object*