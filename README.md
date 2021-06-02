![](https://img.shields.io/vso/build/roblans/df0c356b-e9f5-4364-bdf2-3dde5ed0dc05/7.svg) [![](https://img.shields.io/nuget/v/zwave4net.svg)](https://www.nuget.org/packages/ZWave4Net/)

# ZWave4Net
 ZWave4Net is a .NET library that interfaces with the Aeotec / Aeon Labs Z-Stick. It uses an event-driven, non-blocking model that makes it lightweight and efficient.

Supported Targets:

- .NET 5.0
- .NET 4.8
- .NET Core 2.0
- .NET Core 3.0
- .NET Standard 2.0
- Universal Windows: uap10.0
- Portable Class Library: net45 + win8

Runs on Raspberry PI IoT Windows 10 (see note below)

NuGet package: https://www.nuget.org/packages/ZWave4Net/

Supported Z-Wave command classes:

- Alarm
- Association
- Basic
- Battery
- CentralScene
- Clock
- Color
- Configuration
- ManufacturerSpecific
- Meter
- MultiChannel
- MultiChannelAssociation
- SceneActivation
- Schedule
- SensorAlarm
- SensorBinary
- SensorMultiLevel
- SwitchBinary
- ThermostatFanMode
- ThermostatMode
- ThermostatOperatingState
- ThermostatSetpoint
- Version
- WakeUp
 
FIBARO Wall Plug sample:

```cs
        public async Task TurnWallPlugOn()
        {
            // the nodeID of the wallplug
            byte wallPlugNodeID = 3;

            // create the controller
            var controller = new ZWaveController("COM1");
            
            // open the controller
            controller.Open();

            // get the included nodes
            var nodes = await controller.GetNodes();
            
            // get the wallplug
            var wallPlug = nodes[wallPlugNodeID];
            
            // get the SwitchBinary commandclass
            var switchBinary = wallPlug.GetCommandClass<SwitchBinary>();

            // turn wallplug on
            await switchBinary.Set(0xFF);

            // close the controller
            controller.Close();
        }
```

FIBARO Motion Sensor sample:

```cs
        public async Task SensorAlarm()
        {
            // the nodeID of the motion sensor
            byte motionSensorID = 5;

            // create the controller
            var controller = new ZWaveController("COM1");

            // open the controller
            controller.Open();

            // get the included nodes
            var nodes = await controller.GetNodes();

            // get the motionSensor
            var motionSensor = nodes[motionSensorID];

            // get the SensorAlarm commandclass
            var sensorAlarm = motionSensor.GetCommandClass<SensorAlarm>();

            // subscribe to alarm event
            sensorAlarm.Changed += (s, e) => Console.WriteLine("Alarm");

            // wait
            Console.ReadLine();

            // close the controller
            controller.Close();
        }
```

Note: running ZWave4Net on Raspberry PI IoT Windows 10:

```cs
    // note: opening the serialport by name fails on Windows 10 IoT, use USB vendorId and productId instead
    var controller = new ZWaveController(vendorId: 0x0658, productId: 0x0200);
```
