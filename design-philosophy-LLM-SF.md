# Asteroids Enhanced - LLM Development Specification

Version: 2.1.0 (Single-File Architecture)
Last Updated: June 2025
Purpose: Essential guide for LLMs working on this codebase

## Quick Start for LLMs

1. **Everything is in main.py** - 7,544 lines of intentionally single-file code
2. **Read state like this**: `g_game_state['effects']['screen_shake']`
3. **Find sections with**: `=== [SECTION NAME] ===` markers
4. **When in doubt**: Duplicate code rather than abstract
5. **Always document**: Side effects and globals in function docstrings
6. **Test with**: F1 (toggle interpolation), F2 (show performance stats)

## Primary Architecture Rules

1. **Single file is PERMANENT** - Not technical debt, it's optimal for LLM development
2. **Maximum visibility** - You see ALL code and dependencies at once
3. **Explicit over clever** - `g_game_state['combo']['current']` not `combo`
4. **Globals are good** - When they clarify data flow
5. **Safe failures** - Wrap risky operations in try-except
6. **Copy don't abstract** - Similar code is fine, abstraction confuses

## Living Document

- **Update examples** when code changes
- **Preserve principles** even if implementation evolves
- **Add insights** that help future LLMs
- Examples show current patterns; principles are eternal

## Code Organization

The file has 35 sections marked with `=== [SECTION NAME] ===`. Key sections:

- **Configuration Namespace** - All constants in `Cfg` class
- **Global State Variables** - All globals defined here
- **Extension Points** - Marked with `=== [LLM EXTENSION POINT] ===`
- **Main Game Loop** - Start here to understand flow

## Critical Globals

```python
# State
g_game_state: dict          # ALL mutable game state (see structure below)

# Entities  
g_ship: ShipState          # Player ship
g_asteroids: List[Asteroid]
g_bullets: List[Bullet]    # Player bullets
g_enemy_bullets: List[Bullet]
g_enemies: List[Enemy]
g_powerups: List[PowerUp]

# Visual
g_particle_pool: ParticlePool  # Reusable particles
g_floating_texts: List[FloatingText]
g_stars: List[dict]        # Background stars
g_dust_particles: List[dict]

# Systems
g_spatial_grid: SpatialGrid # Collision optimization
g_text_cache: TextCache    # Cached text surfaces
g_sounds: Dict[str, Sound] # Sound effects
g_controller: Joystick     # Gamepad input

# Display
g_screen_width: int        # Current window size
g_screen_height: int
g_scale_factor: float      # UI scaling
```

## Game State Structure

```python
g_game_state = {
    # Core
    'score': int, 'lives': int, 'level': int, 'game_over': bool, 'paused': bool,
    
    # Resources  
    'crystals': int, 'high_score': int, 'lifetime_crystals': int,
    'achievements_unlocked': set(), 'upgrade_levels': dict,
    
    # Effects
    'effects': {
        'screen_shake': float, 'damage_flash': float, 'level_transition': float,
        'aura_rotation': float, 'pause_menu_alpha': int, # ... etc
    },
    
    # Combat
    'combo': {'current': int, 'timer': float, 'kills': int, 'max': int, 'pulse': float},
    'dash': {'cooldown': float},
    'finisher': {'meter': float, 'ready': bool, 'phase': FinisherPhase, # ... etc},
    
    # Performance  
    'physics_accumulator': float,  # Fixed timestep
    'update_timers': {'ai': 0.0, 'particles': 0.0, 'ui': 0.0, 'effects': 0.0}
}
```

## Core Patterns

### Adding Features
```python
# 1. Add enum
class PowerUpType(Enum):
    NEW_TYPE = "new_type"

# 2. Configure in Cfg
Cfg.powerup_types[PowerUpType.NEW_TYPE] = {"color": (R,G,B), "symbol": "X"}

# 3. Add to state
@dataclass
class ShipState:
    new_type_active: float = 0

# 4. Add effect
POWERUP_EFFECTS[PowerUpType.NEW_TYPE] = lambda: setattr(g_ship, 'new_type_active', duration)
```

### Function Documentation
```python
def function_name(param: type) -> return_type:
    """One line summary.
    
    Side effects:
        Modifies g_game_state['key']
        
    Globals:
        Reads: g_ship
        Writes: g_game_state
    """
```

### Safe Operations
```python
try:
    risky_operation()
except (pygame.error, ValueError) as e:
    print(f"[function_name] Error: {e}")
    return safe_default
```

## Update Order & Performance

### Frame Structure (60 FPS target)
```python
1. handle_events()     # Input
2. update_game_state() # Physics & logic
3. render_frame()      # Draw everything
```

### Multi-Rate Updates
- **60Hz** (every frame): Physics, collisions, player input
- **30Hz**: Particle effects  
- **20Hz**: UI updates, visual effects
- **15Hz**: Enemy AI decisions

This keeps the game responsive while optimizing performance.

## Human Communication

### They say → You do
- "Too slow" → Increase `Cfg.ship_max_speed`
- "Too hard" → Reduce `Cfg.enemy_spawn_chance`
- "Crashes" → Add bounds checking
- "Laggy" → Check particle count

### You report → They understand  
- Describe what changes visually
- Never mention code details
- "Ships now move 20% faster" ✓
- "Changed Cfg.ship_max_speed to 7.68" ✗

## Common Tasks

```python
# Create explosion
create_explosion(x, y, particle_count, color)

# Spawn enemy
g_enemies.append(create_enemy())

# Add score with combo
g_game_state['score'] += points
add_combo()

# Show text
create_floating_text(x, y, "TEXT", color)

# Grant achievement
check_achievement('achievement_id')
```

## Extension Points

Look for these markers:
```python
# === [LLM EXTENSION POINT: Add new PowerUp types here] ===
# === [LLM EXTENSION POINT: Add new enemy AI types here] ===
# === [LLM EXTENSION POINT: Add new achievement conditions here] ===
# === [LLM EXTENSION POINT: Add new powerup effects here] ===
```

## Performance Systems

- **ParticlePool**: Reuses objects, max 1000 active
- **SpatialGrid**: O(n*k) collisions instead of O(n*m)  
- **TextCache**: Avoids re-rendering static text
- **get_sin_cos()**: Caches trig calculations
- **Interpolation**: Smooth 60fps rendering even if physics hiccups

## Key Insights

- **Division by zero**: Always check distances > MIN_SAFE_DISTANCE
- **Screen wrapping**: `wrap_position()` handles edge teleporting
- **State saves**: JSON to `Cfg.save_file`, preserves upgrades
- **Sound optional**: Game works without numpy/sound
- **Debug modes**: F1 = interpolation, F2 = performance stats

## Constraints

- 60 FPS minimum (16ms frame budget)
- Memory < 200MB
- Particles < 1000 active
- Single file (currently 7,544 lines)

---
Remember: You're modifying a complete, working game. Every function documents its side effects. The single-file design means you see everything. Make it better, keep it clear.