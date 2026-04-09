# thor-rom

Jetson ROM image configuration generator. Produces setup scripts, docker-compose, and character sheets for Jetson flash-from-scratch.

## Presets

| Preset | Model | Use Case | Models | Power |
|--------|-------|----------|--------|-------|
| fishing-vessel | Orin Nano | Marine | r1:1.5b, phi3:mini | 15W |
| drone | Orin Nano | Aerial | r1:1.5b | 15W |
| home-assistant | Orin Nano | Home | phi3:mini | 10W |
| industrial | Orin NX | Industrial | r1:1.5b, phi3:mini | 25W |

## Usage

```python
from rom import ROMConfig, JetsonModel, UseCase, generate_setup_script

config = ROMConfig(
    jetson_model=JetsonModel.ORIN_NANO,
    use_case=UseCase.MARINE,
    hostname="fishing-boat-01",
    install_deckboss=True,
    install_vessel_bridge=True,
    install_a2a_r=True,
    preloaded_models=["deepseek-r1:1.5b", "phi3:mini"])

script = generate_setup_script(config)
with open("setup.sh", "w") as f:
    f.write(script)
chmod +x setup.sh
./setup.sh
```

Or use a preset:

```python
from rom import ROM_PRESETS

config = ROM_PRESETS["fishing-vessel"]
script = generate_setup_script(config)
```

## What Gets Installed

- Ubuntu system packages (git, python3, pip, curl, vim)
- Docker + GPU access
- Python 3.11 venv
- Ollama + preloaded models
- Deckboss CLI + onboarding
- Vessel Bridge hardware abstraction
- A2A-R protocol stack
- Hardware watchdog
- Thermal management
- Domain-specific packages (GPS for marine, Gazebo for aerial)

## The One Command

```bash
python3 -c "from rom import ROM_PRESETS, generate_setup_script; open('setup.sh','w').write(generate_setup_script(ROM_PRESETS['fishing-vessel']))" && bash setup.sh
```

Part of the [Lucineer ecosystem](https://github.com/Lucineer/the-fleet).
