## Status File

In addition to the journal file, which is written incrementally, there is now (in v3.0) a new file _**Status.json**_ which is updated every few seconds, with some information about the current state of the game.

This has a similar format to a line in the journal, but the whole file is replaced every time. It has a timestamp like the journal, and "event":"Status"

Parameters:

- Flags: multiple flags encoded as bits in an integer (see below) 
- Flags2: more flags, mainly for when on foot 
- Pips: an array of 3 integers representing energy distribution (in half-pips) 
- Firegroup: the currently selected firegroup number 
- GuiFocus: the selected GUI screen 
- Fuel: { FuelMain, FuelReservoir} – both mass in tons 
- Cargo: mass in tons 
- LegalState 
- Latitude (if on or near a planet) 
- Altitude 
- Longitude 
- Heading 
- BodyName 
- PlanetRadius 
- Balance 
- Destination: 
	- System 
	- Body 
	- Name 


LegalState: one of:

"Clean",

"IllegalCargo",

"Speeding",

"Wanted",

"Hostile",

"PassengerWanted",

"Warrant"

Additional values when on foot:

- Oxygen: (0.0 .. 1.0) 
- Health: (0.0 .. 1.0) 
- Temperature (kelvin) 
- SelectedWeapon: name 
- Gravity: (relative to 1G) 


Flags:

BitValueHexMeaning

010000 0001Docked, (on a landing pad)

120000 0002Landed, (on planet surface)

240000 0004Landing Gear Down

380000 0008Shields Up

4160000 0010Supercruise

5320000 0020FlightAssist Off

6640000 0040Hardpoints Deployed

71280000 0080In Wing

82560000 0100LightsOn

95120000 0200Cargo Scoop Deployed

1010240000 0400Silent Running,

1120480000 0800Scooping Fuel

1240960000 1000Srv Handbrake

1381920000 2000Srv using Turret view

14163840000 4000Srv Turret retracted (close to ship)

15327680000 8000Srv DriveAssist

16655360001 0000Fsd MassLocked

171310720002 0000Fsd Charging

182621440004 0000Fsd Cooldown

195242880008 0000Low Fuel ( &lt; 25% )

2010485760010 0000Over Heating ( &gt; 100% )

2120971520020 0000Has Lat Long

2241943040040 0000IsInDanger

2383886080080 0000Being Interdicted

24167772160100 0000In MainShip

25335544320200 0000In Fighter

26671088640400 0000In SRV

271342177280800 0000Hud in Analysis mode

282684354561000 0000Night Vision

295368709122000 0000Altitude from Average radius

30‭1073741824‬4000 0000fsdJump

3121474836488000 0000srvHighBeam

**Flags2 bits:**

Bitvaluehexmeaning

010001OnFoot

120002InTaxi (or dropship/shuttle)

240004InMulticrew (ie in someone else's ship)

380008OnFootInStation

4160010OnFootOnPlanet

5320020AimDownSight

6640040LowOxygen

71280080LowHealth

82560100Cold

95120200Hot

1010240400VeryCold

1120480800VeryHot

1240961000Glide Mode

1381922000OnFootInHangar

14163844000OnFootSocialSpace

15327688000OnFootExterior

16655360001 0001BreathableAtmosphere

171310720002 0000Telepresence Multicrew

182621440004 0000Physical Multicrew

195242880008 0000Fsd hyperdrive charging

Examples:

```
{
	"timestamp": "2017-12-07T10:31:37Z",
	"event": "Status",
	"Flags": 16842765,
	"Pips": [
		2,
		8,
		2
	],
	"FireGroup": 0,
	"Fuel": {
		"FuelMain": 15.146626,
		"FuelReservoir": 0.382796
	},
	"GuiFocus": 5
}
```

```
{
	"timestamp": "2017-12-07T12:03:14Z",
	"event": "Status",
	"Flags": 18874376,
	"Pips": [
		4,
		8,
		0
	],
	"FireGroup": 0,
	"Fuel": {
		"FuelMain": 15.146626,
		"FuelReservoir": 0.382796
	},
	"GuiFocus": 0,
	"Latitude": -28.584963,
	"Longitude": 6.826313,
	"Heading": 109,
	"Altitude": 404
}
```

In the first example above 16842765 (0x0101000d) has flags 24, 16, 3, 2, 0: In main ship, Mass locked, Shields up, Landing gear down, Docked

GuiFocus values:

0NoFocus

1InternalPanel (right hand side)

2ExternalPanel (left hand side)

3CommsPanel (top)

4RolePanel (bottom)

5StationServices

6GalaxyMap

7SystemMap

8Orrery

9FSS mode

10SAA mode

11Codex

The latitude or longitude need to change by 0.02 degrees to trigger an update when flying, or by 0.0005 degrees when in the SRV

If the bit29 is set, the altitude value is based on the planet's average radius (used at higher altitudes)

If the bit29 is not set, the Altitude value is based on a raycast to the actual surface below the ship/srv