### Project Prompt: "Poly-Sim" - A Unified Simulator of Self-Organizing Networks

#### 1. High-Concept: The Vision

Poly-Sim is an interactive, real-time art application for mobile devices that visualizes a universal principle of self-organization. It simulates the emergence of complex, filamentary networks based on a single, unified algorithm. The user can interact with the simulation and switch between "skins" or presets that frame the same underlying process as different natural phenomena: the growth of a slime mold (Physarum), the behavior of astrophysical plasma, and the formation of neural pathways.

It is not a scientifically accurate simulator, but an artistic and educational tool to build intuition about how simple rules lead to complex, beautiful, and efficient structures.

#### 2. Core Scientific Premise: The "Why"

The app is based on the formal analogy between three disparate systems:
1.  Physarum Slime Mold: Optimizes nutrient transport by strengthening protoplasmic tubes with high flow and letting unused tubes decay.
2.  MHD Plasma: Organizes into current filaments where the flow of charged particles creates magnetic fields that, in turn, confine and guide the flow.
3.  Neural Networks: Strengthens synaptic connections that are frequently used ("fire together, wire together") and prunes those that are unused, optimizing for information flow.

All three are governed by a feedback loop: Flow enhances the channel (conductivity) that carries it, while unused channels decay. Poly-Sim models this core principle.

#### 3. The Core Algorithm: The "How"

The simulation will be implemented as a GPU-based, agent-based model on a 2D grid (a texture).

State Data: The Conductivity Field C
A high-resolution floating-point texture where each pixel's value represents the "conductivity" or "path desirability" at that point.

Flow Data: The Agents
A large array (100k to 1M+) of simple particles, each with a position (x, y) and a direction vector (dx, dy).

The Main Simulation Loop (to be executed entirely on the GPU every frame):

Agent Update Step (Flow Calculation G(C)):
For each agent, read the C value from the texture at its current position and at two other "sensor" points ahead of it (e.g., at current_pos + rotated_direction(sensor_angle) and current_pos + rotated_direction(-sensor_angle)).
Steer the agent's direction vector towards the sensor point with the highest C value.
Add a small random value to the direction to prevent getting stuck.
Move the agent forward according to its new direction vector. Agents that move off-screen should wrap around to the other side.

Reinforcement Step (Feedback R(F)):
For each agent, increase the value of the pixel in the C texture at its new position by a small, fixed "deposit strength". This makes paths stronger where agents have traveled.

Dissipation Step (Decay D(C)):
This step is applied to the entire C texture.
a) Decay: Multiply the entire texture by a constant decay factor (e.g., 0.98). This makes all paths fade over time. C *= decay_rate.
b) Diffusion: Apply a slight blur filter to the texture. This smooths the paths, prevents aliasing, and encourages nearby paths to merge, creating more organic, river-like structures.

#### 4. App Functions & User Experience

Main View: A full-screen, visually captivating rendering of the C field. The pixel value should be mapped to a vibrant color gradient.
Core Interaction:
Touch & Drag: Spawns agents at the touch location. This allows the user to "paint" with flow, acting as a food source or a current injector.
Multi-touch: Use standard gestures for panning and zooming the camera.
Main Control Panel (Minimalist & Collapsible):
Preset Selector: A simple dropdown/button group to switch between [ Physarum | Plasma | Neural ] modes. Each preset will load a pre-configured set of parameters and a unique color scheme.
Key Parameter Sliders:
Agent Count: Controls simulation density.
Sensor Angle: Controls how sharply agents can turn.
Decay Rate: Controls how quickly old paths fade.
Deposit Strength: Controls how quickly new paths are formed.
Reset Button: Clears the screen and restarts the simulation.
Preset Definitions:
Physarum: Slower simulation speed, moderate decay, strong blur/diffusion. An organic yellow/green/brown color palette.
Plasma: High agent speed, high decay rate (more "electric" and transient), minimal blur. An electric blue/purple/white color palette with a strong bloom/glow effect.
Neural: Lower agent count, very low decay (paths are more permanent), agents may be encouraged to turn at sharper angles. A cooler, more clinical blue/cyan color palette.
#### 5. Technical Specifications

Target Platform: iOS and Android, targeting modern devices.
Technology: Recommended: Unity (using VFX Graph or Compute Shaders) or Godot (using Compute Shaders). A high-performance WebGL implementation is also feasible. The core logic must be offloaded to the GPU for real-time performance.
Data Structures:
Conductivity Field: Two floating-point render textures (one for reading, one for writing, swapped each frame).
Agent Data: A GPU buffer (e.g., StructuredBuffer or SSBO) holding agent positions and vectors.
Performance Goal: Maintain 60 FPS with at least 500,000 agents on a mid-range device (e.g., iPhone 12 / Samsung Galaxy S21).