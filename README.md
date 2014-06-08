# gamepad.js

I haven't been able to find any libraries for interacting with the HTML5 Gamepad API that I really like. This is an attempt to sort out some of my ideas for what I would want out of such a library.


## Requirements

- Abstract away complexity
	- Make it dead simple for developers to implement
	- Allow devs to focus on making games, not fiddling with controller config
- Multi player support
	- Handle input from multiple gamepads
	- Don't sacrifice simplicity for single gamepad usage (most common use case)
- Independent controls per player
	- Allow player 1 to have different control settings from player 2
	- Player 1 prefers button A to jump
	- Player 2 prefers right bumper to jump


## Event Driven Approach
	
```
	Gamepad.player(1).on('connect', function () {
		console.log('Player 1 is connected');
	});
	
	Gamepad.player(1).on('buttonpress', function (button) {
		console.log('Player 1 pressed ' + button);
	});
```

**Pros**

- Familiar EventEmmiter pattern

**Cons**

- Verbose
- Conditional testing for which button