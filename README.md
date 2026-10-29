[中文说明](README_CN.md)

# OmniHand Pro 2025 (O12) ROS 2 Package

This is the `omnihand_pro_2025_description` ROS 2 package for **OmniHand Pro 2025 (O12)**. It includes the URDF files for both the left and right hands, along with visualization support in RViz. The recommended ROS 2 distribution for using this package is **Humble**.

## File Structure

```text
omnihand_pro_2025_description/
├── CMakeLists.txt
├── package.xml
├── launch/          # ROS 2 visualization launch files
├── urdf/            # Primitive collision models and xacro/ sources
├── urdf_mesh_col/   # Convex collision models and xacro/ sources
├── meshes/          # STL meshes, with collision meshes in collision/
├── rviz/            # RViz configurations
└── mjcf/            # MuJoCo models and scenes
```

Resource directories are installed under `share/omnihand_pro_2025_description/` with the same layout.
Earlier versions used the ROS package name `omnihand_pro_description` and kept resources under `assets/`. Projects using those versions need to update the package name and source paths. Joint names, model filenames, launch filenames, and model parameters are unchanged.

## Installation

### 1. Clone the Repository
Create a new `src` directory, navigate into it, and clone the repository:

```bash
mkdir -p ~/omnihand_pro_ws/src
cd ~/omnihand_pro_ws/src
git clone <repository_url> omnihand_pro_2025_description
cd ..
```

### 2. Build the Package
Build the workspace using:

```bash
rosdep install --from-paths src --ignore-src -r -y
colcon build
```

### 3. Source the Setup File
Before using the package, source the workspace setup file:

```bash
source install/setup.bash
```

## Generate OmniHandPro URDF File

To generate the URDF description from the Xacro files:

```bash
cd ~/omnihand_pro_ws/src/omnihand_pro_2025_description/urdf
xacro xacro/omnihand_pro_right.xacro > omnihand_pro_right.urdf
xacro xacro/omnihand_pro_left.xacro > omnihand_pro_left.urdf
```

## Visualization in RViz

### Right Hand
```bash
ros2 launch omnihand_pro_2025_description omnihand_pro_description.launch.py
```

### Left Hand
```bash
ros2 launch omnihand_pro_2025_description omnihand_pro_description.launch.py hand_type:=left
```

## High Precision Collision URDF
This package additionally provides a collision-optimized URDF model, generated using convex optimization over original hand mesh geometry.
Compared to standard primitive collision approximations, this version produces more accurate physical interaction.

The convex collision URDF is located at:
omnihand_pro_2025_description/urdf_mesh_col/

### Right Hand With Accurate Collision
```bash
ros2 launch omnihand_pro_2025_description omnihand_pro_description_col.launch.py
```

### Left Hand With Accurate Collision
```bash
ros2 launch omnihand_pro_2025_description omnihand_pro_description_col.launch.py hand_type:=left
```

> **Note on Joint Coupling Accuracy**\
> URDF only supports **linear coupling** between passive and active
> joints.\
> However, OmniHandPro's mechanical structure uses **nonlinear
> coupling** for most passive joints, making it impossible for the URDF
> model to fully match the real hardware behavior.\
> If you need to simulate the **true nonlinear coupling**, please refer
> to:
> - The **MuJoCo MJCF model**, which contains accurate nonlinear mappings
> - The **OmniHandPro SDK**, which provides real coupling functions

------------------------------------------------------------------------

## MuJoCo MJCF Files

This package also provides **MuJoCo MJCF models** for simulating OmniHandPro in physics-based environments.\
The MJCF files are compatible with **MuJoCo 3.1.0 and later**.

Model files are located at:

```
omnihand_pro_2025_description/mjcf/
```

This directory includes:

- `scene.xml` – A complete scene file for quick preview\
- `omnihand_pro_left.xml` – Left-hand model\
- `omnihand_pro_right.xml` – Right-hand model\
- `omnihand_pro_asset.xml` – Mesh resources and simulation defaults

The STL mesh assets are located in `../meshes/`.

### Quick Preview

To quickly preview the OmniHandPro model:

1. Open MuJoCo’s `simulate` viewer\
2. Drag and drop `mjcf/scene.xml` into the window
3. The OmniHandPro model will automatically load and display

Example:

![alt text](mujoco_image.png)
