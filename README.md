# BeamNGpy


[![Documentation](https://github.com/BeamNG/BeamNGpy/raw/master/media/documentation.png)][1]

## Table of Contents
 - [About](#about)
 - [Features](#features)
 - [Prerequisites](#prereqs)
 - [Installation](#installation)
 - [Usage](#usage)
 - [Troubleshooting](#troubleshooting)

<a name="about" ></a>

## About

BeamNGpy is an official library providing a Python API to BeamNG.tech,
the academia- and industry-oriented fork of the video game [BeamNG.drive][4].
BeamNGpy and BeamNG.tech are designed to go hand in hand, both being kept up
to date to support each other's functions, meaning using the latest versions
of both is recommended.

It allows remote control of the simulation, including vehicles contained in it:

<a name="features"></a>

## Features

BeamNGpy comes with a wide range of low-level functions to interact with the
simulation and a few higher-level interfaces that make more complex actions
easier. Some features to highlight are:

### Remote Control of Vehicles

Each vehicle can be controlled individually and independently during the
simulation. This includes basic steering inputs, but also controls over
various lights (headlights, indicators, etc.) or gear shifting.

![Throttle control](https://github.com/BeamNG/BeamNGpy/raw/master/media/throttle.gif) ![Steering control](https://github.com/BeamNG/BeamNGpy/raw/master/media/steering.gif)

### AI-controlled Vehicles

Besides manual control, BeamNG.tech ships with its own AI to control vehicles.
This AI can be configured and controlled from BeamNGpy. It can be used to
make a vehicle drive to a certain waypoint, make it follow another vehicle,
span the map, or follow a user-defined trajectory:

![AI Trajectory](https://github.com/BeamNG/BeamNGpy/raw/master/media/ai_trajectory.png)

### Dynamic Sensor Models

Vehicles and the environment can be equipped with various sensors that provide
simulated sensor data. These sensors include:

 - Cameras
  - Color camera
  - Depth camera
  - Semantic and Instance annotations
 - Lidars
 - Inertial Measurement Units
 - Ultrasonic Distance Measurements

![Multiple cameras](https://github.com/BeamNG/BeamNGpy/raw/master/media/camera.png)
![Lidar](https://github.com/BeamNG/BeamNGpy/raw/master/media/lidar.gif)

These sensors give perfect data from the simulation by default. Therefore, some
of them, like the camera and lidar sensor, can be equipped to also simulate
noisy data.

### Access to Road Network & Scenario Objects

Geometry of roads in the currently-loaded level/scenario are made available
via BeamNGpy. Objects and vehicles that are currently active in the scene
are also exposed, allowing for analysis of the current simulation state.

![Road Network](https://github.com/BeamNG/BeamNGpy/raw/master/media/road_network.png)

### Multiple Clients

BeamNGpy interacts with BeamNG.tech as the client, with BeamNG.tech acting
as the server. This allows for multiple BeamNGpy processes to connect to a
running simulation and have each control the simulator, making it possible
to, for example, [run a scenario in which each vehicle is controlled by
a separate client.](https://github.com/BeamNG/BeamNGpy/tree/master/examples/multi_client.ipynb)

### More

There is a healthy collection of usage examples in the [examples/](https://github.com/BeamNG/BeamNGpy/tree/master/examples)
folder of this repository. These highlight more features, but also serve
as documentation, so be sure to check them out.

<a name="prereqs"></a>

## Prerequisites

Usage of BeamNGpy requires BeamNG.tech to be installed. Builds of
BeamNG.tech are made available for non-commercial use upon request using
[this form][2]. For commercial use, contact us at [licensing@beamng.gmbh][3].
Once downloaded (and extracted, depending on whether or no BeamNG.tech was
obtained as a `.zip`), you can set an environment variable `BNG_HOME` to where
BeamNG.tech can be run from, or provide a path to the BeamNGpy library
during initialization.

The regular [Steam release of BeamNG.drive][4] is compatible to an extent as
well. Certain sensors like the simulated LiDAR or camera will not work, but
most of the functions that are not exclusive to a Tech build will likely
work.

<a name="installation"></a>

## Installation

The library itself is available on [PyPI][5] and can therefore be installed
using common methods like `pip`:

    pip install beamngpy


<a name="usage"></a>

## Usage

**For using BeamNG.tech version 0.22 and above the workspace needs to be set up by the user.
This step needs to be repeated for every newly installed BeamNG.tech version** and helps BeamNGpy to determine the correct user directory for mod deployment.
Create a workspace directory and then place your license file into it. Then type `beamngpy setup-workspace d:\exampleBNGworkspace` into the commandline.
Optional parameters are `--host localhost --port 65255`.
Note that this functionality is only available for BeamNGpy version 1.19.2 and above.

Once set up, the library can be imported using `import beamngpy`. A short
usage example setting up a scenario with one vehicle in the West Coast USA map
that spans the area is:

```python
from beamngpy import BeamNGpy, Scenario, Vehicle

# Instantiate BeamNGpy instance running the simulator from the given path,
# communicating over localhost:64256
bng = BeamNGpy('localhost', 64256, home='/path/to/bng/tech')
# Launch BeamNG.tech
bng.open()
# Create a scenario in west_coast_usa called 'example'
scenario = Scenario('west_coast_usa', 'example')
# Create an ETK800 with the licence plate 'PYTHON'
vehicle = Vehicle('ego_vehicle', model='etk800', licence='PYTHON')
# Add it to our scenario at this position and rotation
scenario.add_vehicle(vehicle, pos=(-717, 101, 118), rot=None, rot_quat=(0, 0, 0.3826834, 0.9238795))
# Place files defining our scenario for the simulator to read
scenario.make(bng)

# Load and start our scenario
bng.load_scenario(scenario)
bng.start_scenario()
# Make the vehicle's AI span the map
vehicle.ai_set_mode('span')
input('Hit enter when done...')
```

We have a [guide][6] helping you getting started and navigating our collection of examples and
the documentation of the library is available [here][7].

## Troubleshooting

This section lists common issues with BeamNGpy in particular. Since this
library is closely tied to BeamNG.tech and thus BeamNG.drive, it is also
recommended to consult the documentation on BeamNG.drive here:

[https://documentation.beamng.com/][8]

### BeamNGpy cannot establish a connection

 - Be sure to complete the initial set up step described in the Usage section and to repeat it with every newly released BeamNG.tech version.
 - Make sure BeamNG.tech and Python are allowed to connect to your current
   network in Windows Firewall.

### BeamNG.tech quietly fails to launch

- There is a known issue where BeamNG.tech quietly crashes when there is a
  space in the configured userpath. Until this issue is fixed, it is
  recommended to either switch to a path that does not contain a space or
  change the userpath directly in the "startup.ini" file located in the
  directory of your BeamNG.tech installation.

## Contributions

We always welcome user contributions, be sure to check out our [contribution guidelines][9] first, before starting your work.

[1]: https://beamngpy.readthedocs.io/en/latest/
[2]: https://register.beamng.tech/
[3]: mailto:licensing@beamng.gmbh
[4]: https://store.steampowered.com/app/284160/BeamNGdrive/
[5]: https://pypi.org/project/beamngpy/
[6]: https://github.com/BeamNG/BeamNGpy/blob/dev/examples/guide.md
[7]: https://beamngpy.readthedocs.io/en/latest/
[8]: https://documentation.beamng.com/
[9]: https://github.com/BeamNG/BeamNGpy/blob/dev/contributing.md
