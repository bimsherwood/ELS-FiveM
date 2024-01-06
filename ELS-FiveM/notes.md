
## How 'elsVehs' is Populated

elsVehs is set in in client/util.lua:12 in the "els:changeLightStage_c" event handler.

The "els:changeLightStage_c" event is broadcast to clients in response to the "els:changeLightStage_s" event.

util.lua:349 'changeLightStage()' triggers "els:changeLightStage_s".

'changeLightStage()' is called from multiple places:
- The "els:setSceneLightState_c" event handler, utils.lua:215
- The "els:setTakedownState_c" event handler, utils.lua:259
- The 'upOneStage()' function, utils.lua:581
- The 'downOneStage()' function, utils.lua:616

### "els:setSceneLightState_c"

This event is broadcast to clients from "els:setSceneLightState_s"

Which is triggered from client.lua:141 in the keyboard/controller override thread keyboard.takedown button is unpressed

### "els:setTakedownState_c"

This event is broadcast to clients from "els:setTakedownState_c"

Which is triggered from client.lua:155 in the keyboard/controller override thread, when the keyboard.takedown button is unpressed

### 'upOneStage()' and 'downOneStage()'

This is called from client.lua:132 (and other places) in the keyboard/controller override thread, when the keyboard.stageChange button is unpressed

### Summary

- A keyboard input is registered that either changes the stage up or down, sets the takedown state, or sets the scene light state
- In any case, the function 'changeLightStage()' is called on the client, which triggers "els:changeLightStage_s"
- The server broadcasts "els:changeLightStage_c" to all clients, mentioning the player originating the trigger
- All clients configure elsVehs for that player's vehicle
- All clients then use elsVehs to decide whether the vehicle is close enough, and if so, renders emergency lights
