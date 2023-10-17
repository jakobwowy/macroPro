global and variable management inspired by https://github.com/jschuh/klipper-macros

## Install

- Clone in config folder

  git clone https://github.com/jakobwowy/macroPro.git
- Extend setup.cfg
  - Customize parameter
  - include macros

- Moonraker Update Manager
```
[update_manager macro-pro]
type: git_repo
origin: https://github.com/jakobwowy/macroPro.git
path: ~/printer_1_data/config/macroPro # UPDATE THIS FOR YOUR PATH!!!
primary_branch: main
is_system_service: False
managed_services: klipper
```

```
[gcode_macro _jm_options]
# These are examples of some likely customizations:
# Height in mm from the bottom of the gantry to the top of the print bed
variable_gantry_height: 38
gcode: # This line is required by Klipper.
# Any code you put here will run at klipper startup, after the initialization
# for these macros.

# This line includes all the standard macros.
[include macroPro/*.cfg]
```

### Prusa Slicer

**Start G-Code:**
SET_PRINT_STATS_INFO TOTAL_LAYER=[total_layer_count]
START_PRINT_CUSTOM T_BED=[first_layer_bed_temperature] T_EXTRUDER=[first_layer_temperature] FILAMENT_DIAMETER=[filament_diameter] PART_HEIGHT={total_layer_count * layer_height} AREA_START={first_layer_print_min[0]},{first_layer_print_min[1]} AREA_END={first_layer_print_max[0]},{first_layer_print_max[1]} NUM_EXTRUDERS=[num_extruders] EXTRUDER=[current_extruder] EJECT=TRUE T_RELEASE=40 

**Parameter**
- FILAMENT_DIAMETER
  - FLOAT: Filament diameter used by the slicer
- Continuous Printing / Part ejection
  - EJECT
    - BOOL: 1 -> EXECUTE EJECT gcode after on END_PRINT, 0 -> no eject (eject part by hand)
  - T_RELEASE
    - FLOAT: Wait for temperature to eject part
  - PART_HEIGHT
    - FLOAT: Height of printed part -> change the z-height for ejection (gantry push on the top of the part)
- KAMP and MultiExtruder: 
  - AREA_START, AREA_END
    - See https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging
  - NUM_EXTRUDERS
    - INT: Number of extruders
  - EXTRUDER:
    - INT: Number of active extruder on start (starts with 0)

**End G-Code:**
; total layers count = [total_layer_count]
END_PRINT
