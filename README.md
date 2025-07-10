# O12Hand ROS 2 Package

This is a ROS 2 package for O12Hand. It includes the URDF files for both the left and right hands, as well as their visualization in RVIZ. The recommended ROS 2 version for installation is **Humble**.

## Installation

### 1. Clone the Repository
Create a new `src` directory, navigate into it, and clone the repository:

```bash
mkdir -p ~/o12hand_ws/src
cd ~/o12hand_ws/src
git clone <repository_url>
cd ..
```

### 2. Build the Package
Run the following command to build the package:

```bash
colcon build
```

### 3. Source the Setup File
After building, source the setup file to use the package:

```bash
source install/setup.bash
```

## Generate OmniHand URDF File
To generate the OmniHand URDF file from the Xacro description, use:

```bash
cd ~/o12_ws/src/o12_description/urdf/xacro
xacro o12_t1_right.xacro > o12_t1_right.urdf
xacro o12_t1_left.xacro > o12_t1_left.urdf
```

## Visualization in RViz
To visualize O12Hand in RViz, run:

```bash
ros2 launch o12_description o12_description_launch.py
ros2 launch o12_description o12_description_launch.py hand_type:=left
```
