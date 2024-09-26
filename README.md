# 2D Rocket Simulation Documentation

## Introduction

This document provides a comprehensive overview of the 2D Rocket Simulation program implemented in pure HTML/CSS/JavaScript. The simulation models the motion of a rocket navigating through space under the influence of gravitational attractors. It incorporates principles from Newtonian mechanics, simulating gravitational forces, rocket thrust, and rotational dynamics. The program uses numerical methods to solve the equations of motion and provides an interactive interface for users to control the rocket's movement.

**Click [here](https://htmlpreview.github.io/?https://raw.githubusercontent.com/computerguided/rocket-sim/refs/heads/main/rocket-sim.html) to run the rocket simulation in your browser.**

---

## Overview of the Simulation

The simulation visualizes a rocket moving in a two-dimensional space populated with gravitational attractors (simulating celestial bodies). The rocket is subject to:

- **Gravitational Forces**: Calculated using Newton's law of universal gravitation.
- **Thrust Forces**: Generated by the rocket's thrusters, allowing it to accelerate forward or backward.
- **Rotational Dynamics**: The rocket can rotate left or right, changing its facing direction.

The simulation uses a camera system that pans to follow the rocket, creating the effect of an infinite space. Background stars are rendered to enhance the visual experience and provide a sense of movement.

---

## Physics Background

### Newton's Laws of Motion

The simulation is based on Newton's three laws of motion:

1. **First Law (Inertia)**: An object remains at rest or in uniform motion unless acted upon by a net external force.
2. **Second Law (F=ma)**: The acceleration of an object is directly proportional to the net force acting upon it and inversely proportional to its mass.
3. **Third Law (Action-Reaction)**: For every action, there is an equal and opposite reaction.

In the simulation, the rocket's acceleration results from the combined forces of gravity and thrust.

### Gravitational Force

The gravitational force between two masses is given by Newton's law of universal gravitation:

$`F_g = G \frac{m_1 m_2}{r^2}`$

Where:

- $F_g$: Gravitational force
- $G$: Gravitational constant
- $m_1, m_2$: Masses of the two objects
- $r$: Distance between the centers of the two masses

In this simulation:

- The attractors have a fixed gravitational influence, and their masses are implicit in the gravitational constant $G$.
- The rocket's mass is considered constant and normalized for simplicity.
- The gravitational force is calculated for each attractor and summed to determine the net gravitational force on the rocket.

#### Calculation of Gravitational Force Components

The gravitational force is a vector pointing from the rocket towards the attractor. Its components along the x and y axes are calculated using:

$`F_{gx} = F_g \frac{\Delta x}{r}`$

$`F_{gy} = F_g \frac{\Delta y}{r}`$

Where:

- $\Delta x, \Delta y$: Differences in x and y coordinates between the attractor and the rocket.
- $r$: Distance between the rocket and the attractor.

### Rocket Thrust

The rocket's thrust is modeled as an additional force that can be applied in the direction the rocket is facing (forward thrust) or opposite to it (backward thrust).

#### Thrust Force Components

The thrust force components are calculated using:

$`F_{tx} = F_t \cos(\theta)`$

$`F_{ty} = F_t \sin(\theta)`$

Where:

- $F_t$: Magnitude of the thrust force (controlled by the user).
- $\theta$: The rocket's facing angle in radians.

### Rotational Dynamics

The rocket's rotation is controlled by adjusting its facing angle:

- **Rotation Speed**: The rate at which the rocket rotates (controlled by the user).
- The angle is updated incrementally based on the rotation speed and user input.

---

## Numerical Methods

### Time-Stepping (Euler Method)

The simulation uses the Euler method, a simple numerical technique for solving ordinary differential equations (ODEs), to update the rocket's velocity and position over discrete time steps.

#### Equations for Velocity and Position Updates

1. **Velocity Update**:

$`\vec{v}_{\text{new}} = \vec{v}_{\text{old}} + \vec{a} \cdot \Delta t`$

Where:

- $\vec{v}_{\text{new}}$: Updated velocity vector.
- $\vec{v}_{\text{old}}$: Previous velocity vector.
- $\vec{a}$: Acceleration vector (from net forces).
- $\Delta t$: Time step duration.

2. **Position Update**:

$`\vec{r}_{\text{new}} = \vec{r}_{\text{old}} + \vec{v}_{\text{new}} \cdot \Delta t`$

Where:

- $\vec{r}_{\text{new}}$: Updated position vector.
- $\vec{r}_{\text{old}}$: Previous position vector.

#### Implementation in Code

- **Time Step ($\Delta t$)**: A constant value set in the simulation (e.g., 0.1 seconds).
- **Acceleration Calculation**: The net force divided by the rocket's mass (mass is normalized to 1 for simplicity).
- **Incremental Updates**: Velocity and position are updated each frame using the Euler method.

---

## Program Structure

### Code Organization

The program is structured into several key components:

1. **Initialization**:
   - Sets up the canvas, context, and UI elements.
   - Defines global variables for the simulation state.

2. **Event Handlers**:
   - Manage user interactions with the controls (e.g., toggling thrusters, rotation).

3. **Physics Calculations**:
   - Compute gravitational and thrust forces.
   - Update the rocket's velocity and position.

4. **Rendering**:
   - Draw background stars, attractors, trajectory, and the rocket.
   - Adjust positions based on the camera to achieve panning.

5. **Animation Loop**:
   - The `update` function recursively calls itself using `requestAnimationFrame` to create an animation loop.

### Key Variables and Constants

- **Particle (Rocket)**:
  - `x`, `y`: Position coordinates.
  - `vx`, `vy`: Velocity components.
  - `angle`: Facing angle in radians.

- **Camera**:
  - `x`, `y`: Position coordinates to offset drawing for panning.

- **Attractors**:
  - Array of objects with fixed positions representing gravitational sources.

- **Stars**:
  - Array of background stars with positions and brightness levels.

- **Simulation Parameters**:
  - `G`: Gravitational constant.
  - `dt`: Time step duration.
  - `thrusterAcceleration`: Magnitude of thrust force (adjustable via UI).
  - `rotationSpeed`: Rate of rotation (adjustable via UI).

### Functions

- **`resetRocket()`**:
  - Resets the rocket's state and recenters the camera.

- **`generateStars(numStars, areaSize)`**:
  - Generates background stars within a specified area.

- **`update()`**:
  - Core function that updates physics, handles user input, and renders the scene.

---

## User Interface

### Controls

- **Thruster Acceleration**:
  - Input field to adjust the magnitude of acceleration when thrusters are activated.

- **Rotation Speed**:
  - Input field to adjust how quickly the rocket rotates.

- **Buttons**:
  - **Reset**: Resets the simulation to the initial state.
  - **Accelerate Forward**: Toggles the forward thruster on or off.
  - **Accelerate Backward**: Toggles the backward thruster on or off.
  - **Rotate Left**: Toggles left rotation on or off.
  - **Rotate Right**: Toggles right rotation on or off.

### Modal Popup

- Provides an initial description of the simulation and instructions.
- Appears when the page is loaded and can be dismissed by clicking **"Start Simulation"**.

---

## Detailed Explanation of Formulas and Numerical Solutions

### Gravitational Force Calculation

For each attractor:

1. **Compute Distance Components**:

   $`\Delta x = x_{\text{attractor}} - x_{\text{rocket}}`$
   
   $`\Delta y = y_{\text{attractor}} - y_{\text{rocket}}`$

3. **Compute Distance and Avoid Division by Zero**:

   $`r^2 = (\Delta x)^2 + (\Delta y)^2 + \epsilon`$

   Where $\epsilon$ is a small constant (e.g., 0.0001) to prevent division by zero when \( r \) is very small.
   <br>

4. **Compute Gravitational Force Magnitude**:

   $`F_g = \frac{G}{r^2}`$

   where the masses are incorporated into $G$ for simplification.
    <br>

5. **Compute Force Components**:

   $`F_{gx} = F_g \frac{\Delta x}{r}`$
   
   $`F_{gy} = F_g \frac{\Delta y}{r}`$

7. **Sum Forces from All Attractors**:

   $`F_{\text{total}_x} = \sum F_{gx}`$
   
   $`F_{\text{total}_y} = \sum F_{gy}`$

### Thrust Force Calculation

When thrusters are active:

1. **Compute Thrust Force Magnitude**:

   - Forward Thrust: $F_t = \text{thrusterAcceleration}$
   - Backward Thrust: $F_t = -\text{thrusterAcceleration}$
    <br>

2. **Compute Thrust Force Components**:

   $`F_{tx} = F_t \cos(\theta)`$
   
   $`F_{ty} = F_t \sin(\theta)`$

    where $\theta$ is the rocket's facing angle in radians.
    <br>

4. **Add to Total Forces**:

   $`F_{\text{total}_x} += F_{tx}`$
   
   $`F_{\text{total}_y} += F_{ty}`$

### Acceleration and Velocity Update

Using Newton's second law ($F = ma$) and assuming unit mass ($m = 1$):

1. **Compute Acceleration**:

   $`a_x = F_{\text{total}_x}`$
   
   $`a_y = F_{\text{total}_y}`$

3. **Update Velocity (Euler Method)**:
   
   $`v_x = v_x + a_x \cdot \Delta t`$
   
   $`v_y = v_y + a_y \cdot \Delta t`$

### Position Update

**Update Position (Euler Method)**:
   
   $`x = x + v_x \cdot \Delta t`$
   
   $`y = y + v_y \cdot \Delta t`$

### Rotation Update

When rotation is active:

**Update Facing Angle**:

   - Rotate Left:

        $\theta = \theta - \text{rotationSpeed}$
        <br>

   - Rotate Right:

        $\theta = \theta + \text{rotationSpeed}$

---

## Educational Insights

- **Numerical Integration**: The simulation demonstrates the use of numerical methods (Euler's method) to solve differential equations that describe motion.
- **Vector Calculations**: It shows how forces and velocities are vectors with both magnitude and direction, requiring component-wise calculations.
- **Physics in Programming**: The program illustrates how physical laws can be translated into code, bridging the gap between theoretical physics and practical implementation.
- **Interactivity and Visualization**: By interacting with the simulation, users can gain intuition about how forces affect motion in a space environment.

---

## Possible Extensions and Experiments

- **Variable Mass**:
  Simulate fuel consumption by decreasing the rocket's mass over time.
  <br>

- **Orbit Prediction**:
  Calculate and display predicted orbital paths based on current velocity and position.
  <br>

- **Navigation Challenges**:
  Create obstacles or targets for the rocket to navigate around or towards.
  <br>

- **Navigation Computer**:
  Implement an autopilot system that can guide the rocket to a specific destination.
  <br>

- **Flight Planning**:
  Allow users to set waypoints for the rocket to follow, creating complex flight paths.

---

## Conclusion

The 2D Rocket Simulation provides an interactive platform to explore fundamental concepts in physics and numerical methods. By simulating gravitational forces and rocket dynamics, users can visualize how forces influence motion in space. The program's structure demonstrates how complex physical systems can be modeled and solved using computational techniques.

---

## References

- **Newton's Laws of Motion**: Understanding the foundational principles governing motion.
- **Euler's Method**: A basic numerical technique for solving ordinary differential equations.
- **Gravitational Physics**: Concepts related to gravitational attraction and celestial mechanics.

---

## Source Code Availability

The full source code of the simulation is included in the script provided earlier. Users are encouraged to explore and modify the code to deepen their understanding of the physics and computational methods involved.

---

Feel free to reach out if you have any questions or need further clarification on any aspect of the simulation or the underlying physics!
