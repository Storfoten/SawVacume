Shelly 2PM Saw / Vacuum Controller

This device automatically starts a shop vacuum when a connected saw or other power tool is running.

- SÅG / SAW – permanently powered through Shelly Output 0. The Shelly monitors the saw's power consumption.
- SUG / VACUUM – connected to Shelly Output 1 and automatically switched.
- Saw consumption > 300 W for 1 second → vacuum ON.
- Saw consumption < 300 W for 30 seconds → vacuum OFF.
- If the saw is restarted during the 30-second delay, the vacuum remains running.

The controller operates entirely locally on the Shelly. Internet, Home Assistant, MQTT, or another server is not required.

Wiring

230 V mains voltage can cause serious injury or fire. Only work on the device while disconnected from mains power. Use appropriately rated cable, connectors, strain relief and enclosure. Protective earth must remain continuous to both output sockets.

The Shelly 2PM is wired so that each socket is supplied by one Shelly output:

Connection| Goes to
Incoming L| Shelly L
Incoming N| Shelly N + N on both sockets
Incoming PE| PE on both sockets
Shelly O0| L on SAW socket
Shelly O1| L on VACUUM socket

PE does not pass through the Shelly. It should be connected directly from the incoming cable to both output cables.

Check the terminal markings on the actual Shelly 2PM before wiring; do not rely solely on this diagram.

Shelly configuration
Connect the Shelly to Wi-Fi and open its web interface.

Output configuration

Configure the two outputs as independent switches rather than a cover/roller-shutter pair.

- Switch 0 = SAW
- Switch 1 = VACUUM

Disable automatic timers or other actions that could independently change these outputs.

Install the script

Open Scripts in the Shelly web interface and create a new script, for example:

"Saw Vacuum Controller"

Paste the conent of "shelly_2pm_script.txt":

Save the script, enable Run on startup, and start it.

Changing the behaviour

The important values are at the beginning of the script:

let THRESHOLD_W = 300;

let TURN_ON_DELAY_MS  = 1000;
let TURN_OFF_DELAY_MS = 30000;

"THRESHOLD_W" determines how much power the saw must consume before it is considered running.

"TURN_ON_DELAY_MS" determines how long the saw must remain above the threshold before the vacuum starts.

"TURN_OFF_DELAY_MS" determines how long the saw must remain below the threshold before the vacuum stops.

Times are specified in milliseconds:

- "500" = 0.5 seconds
- "1000" = 1 second
- "10000" = 10 seconds
- "30000" = 30 seconds
- "60000" = 60 seconds

Testing

Before connecting the saw and vacuum, verify in the Shelly interface that Output 0 stays ON.

Then connect the saw and watch its measured power in the Shelly interface.

When the saw exceeds 300 W for approximately one second, Output 1 should switch ON.

Turn the saw off. Output 1 should remain ON for another 30 seconds and then switch OFF.

If the saw is restarted during those 30 seconds, the pending shutdown is cancelled and the vacuum remains running.

Load limits

Both appliances are supplied through the same mains plug and Shelly, so the combined current of the saw and vacuum must remain within the ratings of the Shelly 2PM, plug, cable, connectors and upstream circuit.

Motors such as saws and vacuum cleaners can also have substantial startup/inrush current. Check the rating of the exact Shelly 2PM model against the actual saw and vacuum before use.
