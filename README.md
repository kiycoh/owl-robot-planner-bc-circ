# Robot Planning with Backward Chaining and Circumscription

Knowledge-representation project that plans robot movements across rooms connected by doors. It models “broken doors” via circumscription (default: doors are not broken unless specified), and demonstrates non‑monotonic reasoning by invalidating previously valid paths when anomalies are introduced.

This notebook is a prototype implementation that can be brought to the field. With ROS 2 integration (navigation/sensors), door‑state I/O, and ontology sync, it can drive real robots and replan online when anomalies occur.

- OWL ontology with Room, Door, Robot, BrokenDoor (OWLReady2).
- Path planning via backward chaining: [`dfs_bc`](owl-robot-planner-bc-circ.ipynb) (also a forward variant: [`dfs_fc`](owl-robot-planner-bc-circ.ipynb)).
- Circumscription: BrokenDoor is modeled as a subclass, not a boolean flag.
- Static scenarios and randomized worlds with anomalies.
- Visual reports and ontology schema (Graphviz).

![Static-schema](schema_ontology-1.png)

## API Overview
- World/ontology
  - [`reset_owl_world`](owl-robot-planner-bc-circ.ipynb): clean default world
  - [`create_robot_ontology`](owl-robot-planner-bc-circ.ipynb): define classes/properties and render schema
  - [`populate_static_map`](owl-robot-planner-bc-circ.ipynb): fixed rooms/doors/robot
  - [`create_random_world`](owl-robot-planner-bc-circ.ipynb): random rooms/doors/robot
- Planning
  - [`dfs_bc`](owl-robot-planner-bc-circ.ipynb): backward-chaining DFS from goal to start
  - [`dfs_fc`](owl-robot-planner-bc-circ.ipynb): forward-chaining DFS from start to goal
  - [`plan_and_print_report`](owl-robot-planner-bc-circ.ipynb): run planning + print + render map
  - [`find_path_and_generate_report`](owl-robot-planner-bc-circ.ipynb): compact path+report for programmatic use
- Visualization
  - [`view_schema_ontology`](owl-robot-planner-bc-circ.ipynb): schema diagram
  - [`visualize_robot_map`](owl-robot-planner-bc-circ.ipynb), [`visualize_found_path`](owl-robot-planner-bc-circ.ipynb)
  - [`create_legend`](owl-robot-planner-bc-circ.ipynb)
- Anomalies
  - [`add_random_anomalies`](owl-robot-planner-bc-circ.ipynb): mark random doors as BrokenDoor

## Ontology Modeling Notes
- Doors are traversable if unlocked and not instances of BrokenDoor.
- Circumscription is simulated by defaulting all doors to the base class Door; anomalies specialize an instance into BrokenDoor.
- Using BrokenDoor as a subclass avoids re-stating frame properties (e.g., links) and supports non‑monotonic updates.

## Requirements
- Python 3.9+
- Packages: owlready2, graphviz, jupyter
- Graphviz system binaries (dot) installed

## Quick start
1. Open the project.
2. Run the cells:
   - World reset: [`reset_owl_world`](owl-robot-planner-bc-circ.ipynb)
   - Ontology creation and schema: [`create_robot_ontology`](owl-robot-planner-bc-circ.ipynb)
   - Static map population: [`populate_static_map`](owl-robot-planner-bc-circ.ipynb)
   - Planning utilities and visualization:
     - [`dfs_bc`](owl-robot-planner-bc-circ.ipynb), [`dfs_fc`](owl-robot-planner-bc-circ.ipynb)
     - [`visualize_robot_map`](owl-robot-planner-bc-circ.ipynb), [`visualize_found_path`](owl-robot-planner-bc-circ.ipynb)
     - [`plan_and_print_report`](owl-robot-planner-bc-circ.ipynb), [`create_legend`](owl-robot-planner-bc-circ.ipynb)

Images saved:
- schema_ontology.png
- robot_map_legend.png
- robot_map_<robot>.png
- Ontology export: robot_planning_map.owl

## Static Scenarios
Run the scenario cells in the notebook:
- Base path search (no anomalies).
- Alternative with a blocked (locked) door.
- Add anomaly by making a door broken:
  - Append BrokenDoor to a door’s types:
- Re-run planning with [`plan_and_print_report`](owl-robot-planner-bc-circ.ipynb).

## Random World Scenario
Use:
- World generation: [`create_random_world`](owl-robot-planner-bc-circ.ipynb)
- Add anomalies: [`add_random_anomalies`](owl-robot-planner-bc-circ.ipynb)
- One-shot scenario runner: [`run_random_world_scenario`](owl-robot-planner-bc-circ.ipynb)
- Path+report helper: [`find_path_and_generate_report`](owl-robot-planner-bc-circ.ipynb)

```python
run_random_world_scenario(num_rooms=100, num_doors=200, num_anomalies=70)
```

## License
Add your preferred license (e.g., MIT) in a LICENSE file.