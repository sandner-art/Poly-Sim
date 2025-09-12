# Poly-Sim: A Unified Simulator of Self-Organizing Networks

[![Test Online](https://img.shields.io/badge/Play-Online-brightgreen?style=for-the-badge)](https://sandner-art.github.io/Poly-Sim/) [![Updates](https://img.shields.io/badge/Updates-blue?style=for-the-badge)](updates.md)

https://sandner-art.github.io/Poly-Sim/
Author: Daniel Sandner, September 2025

Interactive, real-time art application for mobile devices that simulates and visualizes a universal principle of self-organization. It generates and compares complex, filamentary networks based on a single, unified algorithm. 

> The current app is an implementation of a specific case (agent-based approximation of a global system on a 2D plane with simulated geometric effects)

The user can interact with the simulation and switch between "skins" or presets that frame the same underlying process as different natural phenomena: the growth of a slime mold (Physarum), the behavior of astrophysical plasma, and the formation of neural pathways.

> The prototype demonstrates how the same flow-reinforcement principle creates different visual behaviors across the three modes, making it an excellent educational tool for understanding self-organization in nature

## Core Scientific Premise

The app is based on the formal analogy between three disparate systems:
1.  Physarum Slime Mold: Optimizes nutrient transport by strengthening protoplasmic tubes with high flow and letting unused tubes decay.
2.  MHD Plasma: Organizes into current filaments where the flow of charged particles creates magnetic fields that, in turn, confine and guide the flow.
3.  Neural Networks: Strengthens synaptic connections that are frequently used ("fire together, wire together") and prunes those that are unused, optimizing for information flow.

All three are governed by a feedback loop: Flow enhances the channel (conductivity) that carries it, while unused channels decay. Poly-Sim models this core principle.

## The Core Algorithm

The simulation is implemented as a GPU-based, agent-based model on a 2D grid (a texture).

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

## Complete Mobile-Ready Features

### **Mobile-Optimized UI:**
- **Responsive design** that works perfectly in portrait/landscape
- **Touch-friendly controls** with proper sizing for fingers
- **Collapsible control panel** to maximize viewing area
- **Swipe-resistant interface** prevents accidental navigation
- **No-zoom policy** for consistent experience

### **Full Simulation Engine:**
- **Real-time agent-based simulation** with 50K+ agents
- **Flow-reinforced conductivity** - the core principle working perfectly
- **Sensor-based steering** for emergent path formation
- **Decay and diffusion** creating organic, evolving patterns

### **Three Distinct Presets:**
- **🟢 Physarum**: Organic growth, warm green/yellow palette, moderate decay
- **🔵 Plasma**: Electric behavior, cool blue/purple, high energy
- **🔷 Neural**: Persistent pathways, cyan palette, low decay

### **Interactive Features:**
- **Touch & drag** to spawn agent swarms (acts as food source/current injector)
- **Real-time parameter adjustment** with all sliders working
- **Live FPS counter** for performance monitoring
- **Pause/Resume** functionality
- **Instant reset** to restart simulation

### **Performance Optimized:**
- **Canvas 2D rendering** for maximum mobile compatibility
- **Efficient memory management** with typed arrays
- **Adaptive resolution** based on device capabilities
- **60 FPS targeting** with performance monitoring

## How Each Preset Demonstrates Self-Organization

1. **Physarum Mode**: Simulates slime mold foraging - agents seek food sources and reinforce successful paths while unused routes decay
2. **Plasma Mode**: Models electromagnetic filaments - high-energy flow creates self-organizing current channels
3. **Neural Mode**: Represents synaptic strengthening - frequently used pathways persist while unused connections fade

## Mobile Performance

This webapp will run smoothly on modern mobile devices because:
- Uses Canvas 2D (universal support)
- Adaptive resolution scaling
- Efficient CPU-based simulation
- Memory-conscious agent management
- Touch-optimized interactions

The core algorithm successfully demonstrates how **flow enhances the channels that carry it** - the fundamental principle behind slime mold networks, plasma physics, and neural development!

Try it out by touching and dragging to see how the agents create beautiful, efficient networks that emerge from simple rules. Each preset shows the same underlying mathematics expressing itself as completely different natural phenomena.


## License

Poly-Sim is a research and educational tool. The source code is licensed under the [MIT License](LICENSE), allowing broad reuse and modification.

Scientific papers, documentation, and other textual materials in this repository are licensed under the [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/), promoting open research and collaborative development.