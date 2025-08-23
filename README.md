
---

## 🔧 Core Functions

### RF System Functions
- **`voltage_mh(V, r, h, phi, Phis)`** - Calculate multi-harmonic voltage
- **`potential_mh(V, r, h, phi, dE_s, Phis, below_transition)`** - Compute RF potential
- **`hamiltonian_mh(...)`** - Calculate Hamiltonian values
- **`tracking_function(...)`** - Single-turn particle tracking

### Visualization Functions
- **`separatrices_mh_colored(...)`** - Plot colored separatrices with fill
- **`separatrices_mh_contours(...)`** - Plot separatrices as contour lines
- **`plot_potentials_multi_phi_s(...)`** - Multi-potential plotting
- **`dE_to_phi_s(...)`** - Compute synchronous phase

### Utility Functions
- **`generateBunch(...)`** - Create initial particle distributions
- **`get_ylim_from_separatrices(...)`** - Auto-determine energy limits

---

## 🏗️ Classes

### `ParticleBeam`

Main class for particle beam simulation and visualization.

**Key Methods:**
- `plus_one_turn()` - Advance particles by one turn
- `advance_x_turns(n)` - Advance particles by n turns
- `plot_state()` - Plot current particle distribution
- `plot_trajectory(turns)` - Plot particle trajectories
- `plot_all()` - Comprehensive voltage/potential/separatrix plot
- `modify_RF_system()` - Change RF parameters during simulation

**Example:**
```python
beam = ParticleBeam(particles, V, r, h, dE_s, Phis, below_transition, constant, E)
beam.advance_x_turns(500)
beam.plot_state()
```

### `MultiHarmonicTrackAnimation`

Animation class for creating particle tracking movies.

**Key Methods:**
- `run_animation()` - Create and save animation
- `_animate(i)` - Animation frame function (with RF parameter modification support)

**Example:**
```python
animation = MultiHarmonicTrackAnimation(
    particle_beam, 'Animation', iterations=1000, framerate=30,
    modification=[V*1.1, r, h, new_phases, dE_s], modification_turn=500
)
animation.run_animation()
```

---

## 📖 Examples

### Example 1: Single Harmonic System
```python
# Simple single harmonic
r = [1]
h = [1] 
Phis = [0]
dE_s = 0

beam = ParticleBeam(particles, V=2, r=r, h=h, dE_s=dE_s, Phis=Phis,
                   below_transition=True, constant=0.01, E=10)
beam.plot_all()
```

### Example 2: Dual Harmonic Bunch Shortening
```python
# Bunch shortening with 2nd harmonic
r = [1, 0.5]
h = [1, 2]
Phis = [0, 0]  # In-phase for bunch shortening

beam = ParticleBeam(particles, V=2, r=r, h=h, dE_s=0, Phis=Phis,
                   below_transition=True, constant=0.01, E=10)
beam.advance_x_turns(1000)
beam.plot_state()
```

### Example 3: Triple Harmonic with Phase Animation
```python
# Triple harmonic with phase modification
r = [1, 0.9, 0.8]
h = [1, 2, 3]
Phis = [0, np.pi, 0]

# Create animation with phase change at turn 300
run_multi_harmonic_animation(
    particle_beam=beam,
    figname='Triple Harmonic Evolution',
    iterations=600,
    framerate=30,
    name='triple_harmonic_demo',
    modification=[V, r, h, [0, 0, np.pi], dE_s],  # Change 2nd harmonic phase
    modification_turn=300
)
```

---

## 🖼️ Visualization Gallery

### Separatrix Examples

<!-- Placeholder for separatrix images -->
![Single Harmonic Separatrix](images/single_harmonic_separatrix.png)
*Single harmonic system showing classic sinusoidal potential and elliptical separatrix*

![Dual Harmonic Bunch Shortening](images/dual_harmonic_shortening.png)
*Dual harmonic system (2nd harmonic in-phase) creating flattened potential for bunch shortening*

![Dual Harmonic Bunch Lengthening](images/dual_harmonic_lengthening.png)
*Dual harmonic system (2nd harmonic out-of-phase) creating peaked potential for bunch lengthening*

![Triple Harmonic Complex](images/triple_harmonic_complex.png)
*Triple harmonic system showing complex multi-well potential structure*

### Potential Well Variations

![Voltage Comparison](images/voltage_comparison.png)
*Comparison of RF voltage waveforms for different harmonic combinations*

![Potential Evolution](images/potential_evolution.png)
*Evolution of RF potential shape with varying harmonic ratios*

---

## 🎬 Animation Examples

### Basic Particle Evolution

<!-- Placeholder for animation GIFs -->
![Basic Evolution](animations/basic_evolution.gif)
*1000 particles evolving in a single harmonic RF system over 500 turns*

### Bunch Shortening Animation

![Bunch Shortening](animations/bunch_shortening.gif)
*Demonstration of bunch shortening using 2nd harmonic in-phase*

### Dynamic RF Parameter Changes

![Parameter Modification](animations/parameter_modification.gif)
*Animation showing particles adapting to RF parameter changes mid-simulation*

### Multi-Well Dynamics

![Multi-Well](animations/multi_well_dynamics.gif)
*Complex particle dynamics in multi-harmonic system with multiple potential wells*

### Below vs Above Transition

![Transition Comparison](animations/transition_comparison.gif)
*Side-by-side comparison of particle behavior below and above transition energy*

---

## 📐 Parameters Guide

### Essential Parameters

| Parameter | Symbol | Description | Units | Typical Range |
|-----------|---------|-------------|-------|---------------|
| `V` | V₁ | Fundamental RF voltage | V, kV, MV | 0.1 - 100 |
| `r` | rᵢ | Harmonic voltage ratios | - | 0 - 1 |
| `h` | hᵢ | Harmonic numbers | - | 1, 2, 3, ... |
| `Phis` | Φᵢ | Phase offsets | rad | 0 - 2π |
| `dE_s` | ΔE_s | Energy gain per turn | Same as E | -E to +E |
| `eta` | η | Phase slip factor | - | ±0.001 - ±0.1 |
| `beta` | β | Relativistic velocity | - | 0.1 - 0.999 |
| `energy` | E | Particle energy | MeV, GeV, TeV | 1 - 10⁶ |

### Advanced Parameters

| Parameter | Description | Default | Notes |
|-----------|-------------|---------|-------|
| `n_phi` | Phase grid points | 2000 | Higher = smoother separatrices |
| `n_delta` | Energy grid points | 400 | Higher = finer energy resolution |
| `phi_margin_percent` | Phase range margin | 20% | For edge effect handling |
| `outer_fill_alpha` | Separatrix fill transparency | 0.2 | 0 = transparent, 1 = opaque |
| `inner_alpha` | Inner separatrix transparency | 0.35 | For multiple separatrices |

---

## 🔬 Advanced Usage

### Custom Initial Distributions

```python
# Gaussian distribution
phases = np.random.normal(0, 0.3, 1000)
energies = np.random.normal(0, 0.2, 1000)
particles = np.vstack([phases, energies])

# Uniform distribution in phase space area
from scipy.stats import multivariate_normal
mean = [0, 0]
cov = [[0.1, 0.05], [0.05, 0.1]]
particles = multivariate_normal.rvs(mean, cov, 1000).T
```

### Multi-Stage RF Programs

```python
# Define RF program stages
stages = [
    {'turns': 0, 'V': 1.0, 'r': [1, 0.5], 'Phis': [0, 0]},
    {'turns': 200, 'V': 1.5, 'r': [1, 0.8], 'Phis': [0, np.pi/4]},
    {'turns': 400, 'V': 2.0, 'r': [1, 1.0], 'Phis': [0, np.pi/2]},
]

# Apply stages during tracking
for i, stage in enumerate(stages[1:], 1):
    beam.advance_x_turns(stage['turns'] - stages[i-1]['turns'])
    beam.modify_RF_system(stage['V'], stage['r'], h, stage['Phis'], dE_s)
    beam.plot_state()
```

### Batch Processing and Analysis

```python
# Parameter scan
voltage_ratios = np.linspace(0, 1, 11)
results = []

for ratio in voltage_ratios:
    r_scan = [1, ratio]
    beam_scan = ParticleBeam(particles.copy(), V, r_scan, [1,2], 0, [0,0], 
                            below_transition, constant, energy)
    beam_scan.advance_x_turns(500)
    
    # Analyze final distribution
    phase_rms = np.std(beam_scan.state[-1][0])
    energy_rms = np.std(beam_scan.state[-1][1])
    results.append({'ratio': ratio, 'phase_rms': phase_rms, 'energy_rms': energy_rms})

# Plot results
import pandas as pd
df = pd.DataFrame(results)
df.plot(x='ratio', y=['phase_rms', 'energy_rms'])
```

---

## 🤝 Contributing

We welcome contributions! Areas where help is needed:

### 🐛 Bug Reports
- Report issues with specific parameter combinations
- Numerical stability problems
- Animation rendering issues

### 💡 Feature Requests
- Additional RF waveform types (sawtooth, square wave)
- 3D visualization capabilities
- Integration with accelerator design codes

### 📝 Documentation
- More physics background explanations
- Additional example notebooks
- Tutorial videos

### 🔧 Code Improvements
- Performance optimization
- Code refactoring
- Test suite development

**How to Contribute:**
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests and documentation
5. Submit a pull request

---

## 📚 References

### Academic References
1. **CAS - CERN Accelerator School**: "Introductory Course on Accelerator Physics" - Longitudinal Beam Dynamics lectures
2. **H. Wiedemann**: "Particle Accelerator Physics", 4th Edition, Springer (2015)
3. **S.Y. Lee**: "Accelerator Physics", 3rd Edition, World Scientific (2012)
4. **K. Wille**: "The Physics of Particle Accelerators", Oxford University Press (2000)

### Technical Documentation
- **CERN SY-RF-BR Section**: RF system documentation and beam dynamics studies
- **CERN CAS**: Course materials and simulation examples
- **PyAccelerator**: Python accelerator physics library documentation

### Related Software
- **MAD-X**: Methodical Accelerator Design - CERN
- **Elegant**: Accelerator simulation code - ANL
- **BMAD**: Relativistic charged particle simulation library - Cornell
- **PyORBIT**: Space charge simulation toolkit

---

## 📄 License

This educational software is provided under the MIT License. See `LICENSE` file for details.

---

## 📞 Contact

**Author:** Anibal Luciano Pastinante  
**Email:** anibalpastinante@gmail.com  
**Institution:** CERN - European Organization for Nuclear Research

For questions, suggestions, or collaboration opportunities, please don't hesitate to reach out!

---

*This notebook is part of the educational materials developed for the CERN Accelerator School and is intended for educational and research purposes.*
