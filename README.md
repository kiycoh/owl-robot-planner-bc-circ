# Robot Planning with Backward Chaining and Circumscription

Knowledge-representation project that **plans robot movements across rooms connected by doors (possibly blocked)**. It models “broken doors” via circumscription (default: doors are not broken unless specified), and demonstrates non‑monotonic reasoning by invalidating previously valid paths when anomalies are introduced.

This notebook is a *prototype implementation* that can be brought to the field. With ROS 2 integration (navigation/sensors), door‑state I/O, and ontology sync, it can drive real robots and replan online when anomalies occur.

- OWL ontology with Room, Door, Robot, BrokenDoor (OWLReady2).
- Path planning via backward chaining: `dfs_bc` (also a forward variant: `dfs_fc`).
- Circumscription: BrokenDoor is modeled as a subclass, not a boolean flag.
- Static scenarios and randomized worlds with anomalies.
- Visual reports and ontology schema (Graphviz).

> ![static-scenario](https://github.com/kiycoh/owl-robot-planner-bc-circ/blob/main/robot_map_robot.png)
> `bathroom1` is the objective but in this scenario it's unreachable.

## API Overview
- World/ontology
  - `reset_owl_world`: clean default world
  - `create_robot_ontology`: define classes/properties and render schema
  - `populate_static_map`: fixed rooms/doors/robot
  - `create_random_world`: random rooms/doors/robot
- Planning
  - `dfs_bc`: backward-chaining DFS from goal to start
  - `dfs_fc`: forward-chaining DFS from start to goal
  - `plan_and_print_report`: run planning + print + render map
  - `find_path_and_generate_report`: compact path+report for programmatic use
- Visualization
  - `view_schema_ontology`: schema diagram
  - `visualize_robot_map`, `visualize_found_path`
  - `create_legend`

<br>
 
>![legend](https://github.com/kiycoh/owl-robot-planner-bc-circ/blob/main/robot_map_legend.png)
> Map's legend

## Ontology Modeling Notes
- Doors are traversable if unlocked and not instances of BrokenDoor.
- Circumscription is simulated by defaulting all doors to the base class Door; anomalies specialize an instance into BrokenDoor.
- Using BrokenDoor as a subclass avoids re-stating frame properties (e.g., links) and supports non‑monotonic updates.

<br>

> ![onto-schema](https://github.com/kiycoh/owl-robot-planner-bc-circ/blob/main/schema_ontology.png)
> Ontology's legend

## Requirements
- Python 3.9+
- Packages: owlready2, graphviz, jupyter
- Graphviz system binaries (dot) installed

## Quick start
1. Open the project.
2. Run the cells:
   - World reset: `reset_owl_world`
   - Ontology creation and schema: `create_robot_ontology`
   - Static map population: `populate_static_map`
   - Planning utilities and visualization:
     - `dfs_bc`, `dfs_fc`
     - `visualize_robot_map`, `visualize_found_path`
     - `plan_and_print_report`, `create_legend`

## Scenarios
- You can run a **static scenario** :
  - Base path search (no anomalies).
  - Alternative with a blocked (locked) door.
  - Add anomaly by making a door broken:
    - Append BrokenDoor to a door’s types:
  - Re-run planning with `plan_and_print_report`.


- You can also run a **radomically created scenario**:
  - World generation: `create_random_world`
  - Add anomalies: `add_random_anomalies`
  - One-shot scenario runner: `run_random_world_scenario`
  - Path+report helper: `find_path_and_generate_report`

```python
run_random_world_scenario(num_rooms=100, num_doors=200, num_anomalies=70)
```

## License
MIT


