# 3D Off-road Terrain Mapping for Autonomous Ground Vehicle  
### *Energy-Optimal Path Planning*  
**Author:** Nguyen Nguyen  
**Affiliation:** University of North Carolina at Charlotte, Department of Mechanical Engineering and Engineering Science  

---

## Overview
This project presents a **3D off-road terrain mapping and energy-optimal path planning system** for an **Autonomous Ground Vehicle (AGV)** using the **Clearpath Jackal** platform.  

The system enables the AGV to:
- Collect **onboard GPS, IMU, and gyroscope data** while traversing unknown outdoor terrain.
- Generate a **3D digital elevation map** from raw sensor data.
- Implement a **cost-aware A*** path planning algorithm that minimizes both **travel distance** and **energy consumption**.
  
The approach integrates field data collection, data optimization, terrain modeling, and pathfinding into a cohesive workflow for **energy-efficient navigation in unstructured environments**.

---

## Objectives
- Develop a **terrain-aware mapping pipeline** using GPS and IMU data collected from the Jackal robot.  
- Construct a **3D digital elevation map** from sparse measurements via **barycentric interpolation**.  
- Design a **cost function** that incorporates both **distance** and **altitude-based energy consumption**.  
- Implement and evaluate **A*** for 3D path planning under energy constraints.  
- Compare **uniform grid sampling** and **randomized sampling** methods for map node generation.

---

## System Architecture

### **Hardware**
- **Platform:** Clearpath Jackal AGV  
- **Primary Sensor:** Swift Navigation Duro GPS (high-precision multi-frequency GNSS)  
- **Additional Sensors:** IMU, gyroscope (for orientation and motion data)  
- **Power System:** Onboard battery with ~2-hour runtime  
- **Processing Unit:** Embedded Ubuntu computer with ROS Noetic  

### **Software**
- **Operating System:** Ubuntu (ROS Noetic)  
- **Framework:** Robot Operating System (ROS)  
- **Languages:** Python, C++  
- **Tools:**  
  - `rosbag` for data recording  
  - `rosbag info` / `rosbag play` for replay and analysis  
  - CSV export tools for post-processing  
  - Visualization with Matplotlib and RViz  

---

## Methodology

### **1. Data Collection**
Sensor data (GPS, IMU) are recorded via ROS and stored in `.bag` files.  
Raw GPS coordinates (R = (θ, λ, h)) are converted into **UTM coordinates** (Easting, Northing, Altitude) for Cartesian processing.  

### **2. Data Processing**
- Duplicate points removed to optimize computation.  
- Coordinates transformed to UTM space.  
- Data filtered and exported as **CSV tables** for visualization and analysis.

### **3. Terrain Reconstruction**
- **Linear barycentric interpolation** used to estimate elevation across unknown regions.  
- **Delaunay triangulation** minimizes interpolation error and builds a mesh for 3D visualization.  
- Result: a dense, 3D surface map from sparse GPS samples.  

### **4. Cost Function**
The cost between nodes incorporates both distance and slope (energy factor):  

\[
\text{Cost} = (1 + e^{\alpha \cdot \Delta h}) \cdot D
\]

Where:  
- D = 2D Euclidean distance between nodes  
- Δh = altitude difference  
- α = weight parameter for elevation energy impact  

### **5. Sampling Strategies**
- **Randomized Sampling:** Uniformly distributed points across terrain.  
- **Uniform Grid Sampling:** Systematic sampling on grid nodes (improves coverage, reduces runtime).  

### **6. Path Planning**
A terrain-aware **A*** algorithm searches the interpolated 3D graph for the energy-optimal path.  

\[
f(n) = g(n) + h(n)
\]

Where:  
- g(n): Actual cost from start to node  
- h(n): Heuristic (2D + energy) cost to goal  

---

## 📊 Results Summary

| Sampling Method | Avg. Runtime | Path Cost | Performance |
|-----------------|--------------|------------|--------------|
| Randomized Points | Slower (esp. >1000 pts) | Slightly less consistent | High variability |
| Grid Sampling | ~3× faster at large samples | More stable | Best accuracy-efficiency balance |

- The **A*** path avoided steep terrain, favoring flatter, energy-efficient routes.  
- Terrain estimation from limited data was effective using barycentric interpolation.  
- Optimal sample size: **≈1000 points** for balanced accuracy and runtime.  

---

## Key Insights
- 3D terrain mapping enables AGVs to **predict and avoid high-energy terrain**.  
- Integrating elevation into path cost functions leads to **smarter, energy-efficient navigation**.  
- Uniform sampling is more computationally scalable for real-time implementation.  

---

## Future Work
- Integrate real-time **LiDAR** and **stereo vision** for higher map fidelity.  
- Implement onboard **online map generation** (reduce post-processing).  
- Expand to **multi-robot coordination** for distributed mapping.  
- Integrate **machine learning** for adaptive terrain classification and prediction.  

---
---

## References
1. Barycentric Weight Interpolation Methods  
2. Hart, P.E., Nilsson, N.J., Raphael, B. (1968). *A Formal Basis for the Heuristic Determination of Minimum Cost Paths.* IEEE Trans. Sys. Sci. Cybernetics.  
3. Kavraki, L.E. et al. (1996). *Probabilistic Roadmaps for Path Planning in High-Dimensional Configuration Spaces.* IEEE Trans. Robotics & Automation.  
4. NASA Jet Propulsion Laboratory. *Curiosity Rover Mission Overview.*  
5. Truong, T., Hong, S. (2020). *3D Terrain-Based Path Planning Using A*** Algorithm.*  

---

## Contact
**Nguyen Nguyen**  
Undergraduate Student – Mechanical Engineering  
University of North Carolina at Charlotte  
📧 Email: [nnguye90@charlotte.edu](mailto:nnguye90@charlotte.edu)
