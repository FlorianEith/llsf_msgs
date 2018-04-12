# llsf_msgs

This ROS package will locate the protobuf messages from the RoboCup Logistics League (RCLL) refbox and build them.
They are then installed in your catkin workspace.

## Getting Started

### Prerequisites

The following software is required to run this package:

ROS Indigo or newer: (e.g. sudo apt-get install ros-indigo-desktop-full), see [ROS Indigo Install Ubuntu](http://wiki.ros.org/indigo/Installation/Ubuntu) for installation process

Installed RCLL refbox (see [RCLL refbox installation](https://trac.fawkesrobotics.org/wiki/RCLLRefBox/Install))

The following ROS packages are required to run this package:
- [protobuf_comm](https://github.com/ethflo/protobuf_comm)

### Installing

Install the required software, create a catkin workspace (see [create ROS catkin workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace)) and download the required ROS packages into the workspaces src folder.

Adjust the llsf_msgs and protobuf_comm packages to your system. Therefore set the REFBOX_ROOT_DIR path variable in the CMakeLists.txt file to your refbox installation path.
```
(in CMakeLists.txt)
...
set(REFBOX_ROOT_DIR /home/username/llsf-refbox)
...
```
Restart your system. For some reasons the refbox header files are just found by ROS build process after a restart of the system.

Build your catkin workspace.

```
cd ~/catkin_ws
catkin_make
```
The RCLL protobuf messages can now be included as header files, e.g:
```
#include <llsf_msgs/Time.pb.h>
```

## Versioning

For the versions available, see the [github repository](https://github.com/ethflo/llsf_msgs).

## Authors

* **Florian Eith** - *Initial work* - [Florian Eith](https://github.com/ethflo)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.
