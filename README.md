# Infinite Flight Connect API Docs & Samples
## Sample apps
- [Offical demo](https://github.com/mlaban/IFCTest/tree/master/Infinite%20Flight%20Connector%20API)
- [Liveflight Connect](https://github.com/LiveFlightApp/Connect-Windows)

## Connection

 1. To enable Infinite Flight command server, check `Enable Infinite Flight Connect` in `Settings > General`
 2. Infinite Flight will broadcast UDP packets on port `15000` containing its own IP address and Port.
Example message :
`{ "Address" : "192.168.0.11", "Port" : 10111 }`

 3. You must then establish a TCP connection on this given host and port

## Get Airplane State

This special command will request the airplane state from Infinite Flight. Response will be received on the same socket :
`{ "Command": "Airplane.GetState", "Parameters": []}`

## Command Messages

A command message is a object of the follwing form :


There are two types of command messages :
 - Commands : `{ "Command": "Commands.{CommandName}", "Parameters": []}`
 - Axis : `{ "Command": "NetworkJoystick.{AxisCommandName}", "Parameters": []}`


### List of commands

#### Joystick

Joystick axis use the `NetworkJoystick.SetAxisValue` command with two params :

 - Axis Name : `0` for Roll, `1` for Pitch
 - Value : a value between `-1024` and `1024`

Example :
```
{
  "Command": "NetworkJoystick.SetAxisValue",
  "Parameters": [ { "Name": 0, "Value": -340 } ]
}
```


#### Plane Systems

Commands to control various systems of the plane. Example, lower flaps down :
```
{
  "Command": "Commands.FlapsDown",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `Brakes` | Toggle brakes on/off |
| `ParkingBrakes` | Toggle parking brakes |
| `FlapsDown` | Decrement flaps |
| `FlapsUp` | Increment flaps |
| `FlapsFullDown` | Set flaps full |
| `FlapsFullUp` | Set flaps clean |
| `Aircraft.SetFlapState` | TODO : provide example |
| `Spoilers` | Switch between spoilers states (Off, Flight, Armed) |
| `LandingGear` | Toggle landing gear  |
| `Pushback` | Toggle Pushback (on/off) |
| `ReverseThrust` | Enable reverse (?) |
| `RollLeft` | |
| `RollRight` | |
| `PitchUp` | |
| `PitchDown` | |
| `ResetCommands` | |
| `ElevatorTrimUp` | Trim elevator up |
| `ElevatorTrimDown` | Trim elevator down |
| `ThrottleUpCommand` | |
| `ThrottleDownCommand` | |

#### Lights

Following commands toggle the state of a light. Example :
```
{
  "Command": "Commands.LandingLights",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `LandingLights` | Toggle landing lights |
| `TaxiLights` | Toggle taxi lights? |
| `StrobeLights` | Toggle strobe lights |
| `BeaconLights` | Toggle beacon lights |
| `NavLights` | Toggle nav lights  |

#### Camera
##### Camera Commands

Following commands can be used to control cameras. Example :
```
{
  "Command": "Commands.NextCamera",
  "Parameters": []
}
```

*For camera moves, see Axis commands below*

| Command  | Description  |
|---|---|
| `ToggleHUD` | Switch between HUD states (Full, Without map, disabled) |
| `NextCamera`| Switch to next camera |
| `PrevCamera` | Switch to previous camera |
| `CameraMoveLeft` | |
| `CameraMoveRight` | |
| `CameraMoveDown` | |
| `CameraMoveUp` | |
| `CameraMoveHorizontal` | |
| `CameraMoveVertical` | |
| `CameraZoomIn` | |
| `CameraZoomOut` | |
| `SetCockpitCamera` | |
| `SetVirtualCockpitCameraCommand` | |
| `SetFollowCameraCommand` | |
| `SetFlyByCamera` | |
| `SetOnboardCameraCommand` | |
| `SetTowerCameraCommand` | |

##### Camera Axis

Camera POV movements can be controlled via the following command :
```
{
  "Command": "NetworkJoystick.SetPOVState",
  "Parameters": [
    { "Name": "X", "Value": 0 },
    { "Name": "Y", "Value": 0 }
  ]
}
```

X and Y values can be either `-1`, `0` or `1`: they determine if the camera will move on each axis, either negatively or positively (or stay still on the given axis with the `0` value). For example, to move the POV to the left only horizontaly, use the following command :

```
{
  "Command": "NetworkJoystick.SetPOVState",
  "Parameters": [
    { "Name": "X", "Value": -1 },
    { "Name": "Y", "Value": 0 }
  ]
}
```

#### ATC

**Live only**
Allows you to send messages to ATC according to the options available on the ATC window. Example, call the ATC command #3 (as shown on the ATC window) :
```
{
  "Command": "Commands.ATCEntry3",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `ShowATCWindowCommand` | Show / Hide the ATC window |
| `ATCEntry1` | Choose the option #1 (or back)  |
| `ATCEntry2` | ... |
| `ATCEntry3` |  ...|
| `ATCEntry4` |  ...|
| `ATCEntry5` |  ...|
| `ATCEntry6` |  ...|
| `ATCEntry7` |  ...|
| `ATCEntry8` |  ...|
| `ATCEntry9` |  ...|
| `ATCEntry10` |  ...|
| `Live.SetCOMFrequencies` | TODO : describe this |

#### Autopilot and Flight Plan

Commands to set the value of an Autopilot param, or to toggle its state.
Examples :
Set HDG param to 270 :
```
{
  "Command": "Commands.Autopilot.SetHeading",
  "Parameters": [{ "Value": 270 }]
}
```

Enable HDG :
```
{
  "Command": "Commands.Autopilot.SetHeadingState",
  "Parameters": [{ "Value": true }]
}
```

Toggle Autopilot :
```
{
  "Command": "Commands.Autopilot.Toggle",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `Autopilot.Toggle` | Toggle autopilot |
| `Autopilot.SetState` | |
| `Autopilot.SetHeading` | Heading |
| `Autopilot.SetAltitude` | Altitude |
| `Autopilot.SetVS` | Vertical Speed |
| `Autopilot.SetSpeed` | Speed |
| `Autopilot.SetHeadingState` | |
| `Autopilot.SetAltitudeState` | |
| `Autopilot.SetVSState` | |
| `Autopilot.SetSpeedState` | |
| `Autopilot.SetApproachModeState` | |
| `FlightPlan.AddWaypoints` | Add waypoints to flightplan |
| `FlightPlan.Clear` | Clear flightplan |
| `FlightPlan.ActivateLeg` | |

### Simulator Commands

Control on simulator. Example, toggle play/pause :
```
{
  "Command": "Commands.TogglePause",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `TogglePause` | |
| `TakeScreenshot` | |
| `ToggleUber` | |
| `ToggleTime` | |
| `ToggleShader` | |
| `ToggleNormal` | |
| `Start Recording` | |
| `Stop Recording` | |

