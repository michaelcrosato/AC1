# Asteroids Enhanced

A modern, feature-rich implementation of the classic Asteroids game built in Python using Pygame. This enhanced version includes advanced mechanics, visual effects, and performance optimizations.

## 🌟 Features

### Core Gameplay
- **Classic Asteroids mechanics** with modern enhancements
- **Progressive difficulty** with dynamic asteroid spawning
- **Power-up system** (Rapid Fire, Triple Shot, Shield, Extra Life, Crystals)
- **Enemy AI ships** with different behavior patterns (Hunter, Circler)
- **Boss asteroids** with increased health and special drops
- **Combo system** with multiplier rewards

### Advanced Mechanics
- **Dash system** with trail effects and invulnerability frames
- **Finisher moves** - special dash attacks against enemies
- **Upgrade system** using collected crystals (Damage, Fire Rate, Speed, Dash Cooldown)
- **Achievement system** with crystal rewards
- **Persistent save system** for progress and unlocks

### Visual & Audio
- **Particle effects** with multiple types and optimized rendering
- **Screen shake** and visual feedback
- **CRT-style effects** with scanlines and vignette
- **Dynamic starfield** with parallax scrolling
- **Procedural sound generation** for retro-style audio
- **Smooth animations** and trail effects

### Performance Optimizations
- **Spatial grid collision detection** (O(n*k) instead of O(n*m))
- **Memory-optimized bullet trails** 
- **Object pooling** for particles
- **Text caching system**
- **Race condition prevention** for window resizing

## 🎮 Controls

### Keyboard
- **Arrow Keys / WASD**: Move ship
- **Spacebar**: Shoot
- **Shift**: Dash / Finisher move
- **P**: Pause
- **M**: Toggle sound
- **R**: Restart (when game over)
- **U**: Upgrade menu (when paused)

### Controller Support
- Full Xbox/PlayStation controller support
- Analog stick movement with deadzone
- Button mapping for all functions

## 🛠️ Technical Details

### Architecture
- **Single-file design** optimized for LLM analysis and modification
- **7000+ lines** of well-documented Python code
- **Modular systems** with clear separation of concerns
- **Configuration-driven** with centralized `Cfg` class

### Performance Features
- **Spatial Grid System**: Optimized collision detection using spatial partitioning
- **Memory Management**: Efficient bullet trail handling and object cleanup
- **Thread Safety**: Race condition prevention for UI operations
- **Scalable Rendering**: Dynamic scaling support for different screen sizes

### Code Quality
- **Type hints** throughout the codebase
- **Comprehensive documentation** with docstrings
- **Error handling** with graceful degradation
- **Configurable settings** for easy gameplay tuning

## 📋 Requirements

- Python 3.7+
- Pygame 2.0+
- NumPy (for sound generation)

## 🚀 Installation & Running

1. **Install dependencies:**
   ```bash
   pip install pygame numpy
   ```

2. **Run the game:**
   ```bash
   python main.py
   ```

## 🎯 Recent Improvements

### Phase 1: Critical Bug Fixes ✅
- Fixed memory leak in bullet trail system
- Resolved race condition in window resize handling
- Fixed JSON serialization for save system

### Phase 2: Performance Optimization ✅ 
- Implemented spatial grid collision detection (10-50x performance improvement)
- Optimized ship collision detection
- Fixed enum serialization issues

### Planned: Phase 3
- Code quality improvements and function decomposition
- Additional performance monitoring
- Magic number extraction

## 🎮 Gameplay Tips

- **Collect crystals** to upgrade your ship between levels
- **Use dash strategically** - it provides invulnerability frames
- **Target enemies with finisher moves** for bonus rewards
- **Build combos** by destroying objects quickly for score multipliers
- **Boss asteroids** require multiple hits but drop valuable crystals

## 🔧 Configuration

The game is highly configurable through the `Cfg` class in `main.py`. You can adjust:
- Gameplay mechanics (speeds, health, damage)
- Visual effects (particle counts, colors, screen effects)
- Audio settings (volumes, sound generation parameters)
- Performance settings (collision grid size, particle limits)

## 📈 Performance Metrics

- **Collision Detection**: O(n*k) complexity with spatial grid (vs O(n*m) naive)
- **Memory Usage**: Optimized bullet trails and object pooling
- **Frame Rate**: Stable 60 FPS with 100+ objects on screen
- **Scalability**: Dynamic resolution support from 400x300 to 4K+

## 🏗️ Architecture Highlights

- **Single-file design** for easy modification and analysis
- **Event-driven systems** with clean state management  
- **Modular rendering pipeline** with effect layers
- **Extensible achievement and upgrade systems**
- **Robust save/load with validation**

## 📜 License

This project is open source. Feel free to use, modify, and distribute.

---

**Asteroids Enhanced** - A modern take on a timeless classic! 🚀✨
