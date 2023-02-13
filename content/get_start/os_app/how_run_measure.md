---
title: "Run the measure"
date: 2022-09-21T14:52:25-07:00
weight: 213
draft: false
pre: "<b>- </b>"
---

## How to run the measure

Before running the measure, review the model requirements in
[Model Requirements](/BEM-for-PRM/user_guide/model_requirements)

<!-- - Select "Components & Measures" from the top menu in the screen
- Select Whole Building/ Space Types in the Apply Measure window
  - Click the arrow next to Space types to show available measures
  - Select the "Create ASHRAE 90.1-2019 PRM Model" measure
  - Make selections in the right-hand side of the window as needed
  - Click on the Apply Measure at the bottom of the screen
   -->

- Navigate to the *_Measures_* tab from the vertical list on the left side of the interface. 
- Select the Create *_ASHRAE 90.1-2019 PRM Model_* measure under *_Whole Building > Space Types_* from the library (right side) and drag it into the *_OpenStudio Measures_* section (left side).
- After dropping the measure, click on the measure and enter/adjust the measure inputs/arguments under the *_Edit_* tab (right side). A detailed description of the arguments can be found in the [API document](https://pnnl.github.io/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/).
- Navigate to the *_Run Simulation_* tab to execute the simulation. 

![Openstudio download measure](/BEM-for-PRM/get_start/os_app/images/osapp3.jpg?width=800px&align=left&classes=border)

![Openstudio download measure](/BEM-for-PRM/get_start/os_app/images/osapp4.jpg?width=800px&align=left&classes=border)

{{% notice info %}}
The measures selected on this tab will not run until you run your model, unlike the *_Apply Measure Now_* option. 
{{% /notice %}}
 

