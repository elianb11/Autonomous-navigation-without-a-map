# Autonomous Vehicle Navigation System Without a Map

As part of our VA50 project at UTBM, we developed a navigation system for an autonomous vehicle that eliminates the need for a high-resolution map, aiming to maximize autonomy. Currently, certain operations still require mapping and general-purpose positioning (high resolution is not necessary).

## Dependencies

This project requires Ubuntu 18.04 with ROS Melodic or Ubuntu 20.04 with ROS Noetic, Python 3.6+ (even with Melodic), and `catkin_tools` (install via `apt install python3-catkin-tools`). The required Python modules are defined in `requirements.txt` and are automatically installed during the `./build_circulation.sh init` step.

## Installation

Compilation and installation of the project are managed via a shell script, `build_circulation.sh`, which automatically installs dependencies and handles differences between ROS Melodic and Noetic.

1. Clone the repository or copy its contents into an existing Catkin workspace. The project uses `catkin_tools` for installation, so a workspace already compiled with `catkin_make` is incompatible and must be reset beforehand. A Melodic workspace not configured for Python 3 also needs to be reset (delete `build/` and `devel/`), as Catkin does not support changing additional CMake parameters after the workspace is initialized.
2. Run `./build_circulation.sh init` to initialize the workspace if necessary and install dependencies.
3. Run `./build_circulation.sh build` to compile the packages.
4. Run `./build_circulation.sh cython` to compile and install the Cython modules (these are unfortunately incompatible with Catkin's build system).

The last step must be repeated after any changes to the Cython modules.

## Project Contents

The project consists of four ROS packages:

- `circulation`: Manages trajectory building and transformations.
- `control`: Handles vehicle control.
- `trafficsigns`: Detects vertical traffic signs.
- `direction`: Allows direct input of directions for intersections via the console.

## Execution

Once all necessary modules are compiled and installed, the project includes five ROS nodes:

- **Transformation Service** (required by all other modules): `rosrun circulation TransformBatch.py circulation4-2.yml`
- **Trajectory Building**: `rosrun circulation circulation4.py circulation4-2.yml road_network.json`
- **Sign Detection**: `rosrun trafficsigns distance_extractor.py circulation4-2.yml`
- **Vehicle Control**: `rosrun control control.py circulation4-2.yml`
- **Intersection Direction Testing Interface**: `rosrun direction signTransmit.py`

The project uses a single YAML parameter file:

- `circulation4.yml` for the simulator version from 10/31/2022.
- `circulation4-1.yml` for the version from 12/27/2022.
- `circulation4-2.yml` for the version from 01/03/2023.

Visibility changes between versions necessitate modifying some predefined information (region of interest, a few fuzzy logic parameters, etc.).
