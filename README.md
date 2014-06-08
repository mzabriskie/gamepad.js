# gamepad.js

I haven't been able to find any libraries for interacting with the HTML5 Gamepad API that I really like. This is an attempt to sort out some of my ideas for what I would want out of such a library and ultimately implement it.


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
	
Gamepad.player(1).on('buttonpress', function (which, value) {
	if (which === Gamepad.Buttons.A) {
		console.log('Player 1 pressed A');
	}
});

Gamepad.player(1).on('axeschange', function (which, value) {
	if (which === Gamepad.Axes.LEFT_ANALOGUE_FORWARD) {
		console.log('Player 1 forward motion left analogue');
	}
});
```

**Pros**

- Familiar EventEmmiter pattern

**Cons**

- Verbose
- Conditional testing for which button/axes triggered event

## Mapping Approach

```
Gamepad.player(1).listen({
	Gamepad.Events.CONNECT: function () {
		console.log('Player 1 is connected');
	},
	Gamepad.Buttons.A: function (value) {
		console.log('Player 1 pressed A');
	},
	Gamepad.Axes.LEFT_ANALOGUE_FORWARD: function (value) {
		console.log('Player 1 forward motion left analogue');
	}
});
```

**Pros**

- No need for conditional testing for which
- Easier to support custom mapping for each user

**Cons**

- ?

## Player Management

For sake of example, let's assume the event driven approach.

```
// Reference players by index
Gamepad.player(1).on('connect', function () { /*...*/ });
Gamepad.player(2).on('connect', function () { /*...*/ });

// Terse reference to player 1 for single gamepad usage
Gamepad.on('connect', function () { /*...*/ });
```