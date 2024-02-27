---
title: "Run the measure"
date: 2022-09-21T14:52:25-07:00
weight: 213
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

#### C. Run the measure in OS App

Before running the measure, review the model requirements in
[Model Requirements](/BEM-for-PRM/user_guide/model_requirements)

<!-- - Select "Components & Measures" from the top menu in the screen
- Select Whole Building/ Space Types in the Apply Measure window
  - Click the arrow next to Space types to show available measures
  - Select the "Create ASHRAE 90.1-2019 PRM Model" measure
  - Make selections in the right-hand side of the window as needed
  - Click on the Apply Measure at the bottom of the screen
   -->

{{<mermaid align="center">}}
graph LR;
A(1.Navigate to Measures tab) --> B(_2. Optional Step_ Check measure directory)
B --> C(3.Select measure)
C --> D(4.Adjust measure inputs)
D --> E(5.Run simulation)
{{</mermaid>}}

1. Navigate to the **_Measures_** tab from the vertical list on the left side of the interface.
2. _Optional: Check that the measure exists in the local measures directory by clicking on the my folder at the bottom right corner._
3. Go to the Library (right side). Under **_Whole Building > Space Types_**, select the **_Create ASHRAE 90.1-2019 PRM Model_** measure and drag it into the **_OpenStudio Measures_** section (left side).
4. After dropping the measure, click on the measure and enter/adjust the measure inputs/arguments under the **_Edit_** tab (right side). Upload the user data .csv files here (optional). The use of the user data can be found in the [add compliance data section](../../../user_guide/add_compliance_data/). A detailed description of the arguments can be found in the [API document](https://pnnl.github.io/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/).
5. Navigate to the **_Run Simulation_** tab to execute the simulation.

![Openstudio download measure](/BEM-for-PRM/get_start/os_app/images/osapp3.jpg?width=800px&align=left&classes=border)

![Openstudio download measure](/BEM-for-PRM/get_start/os_app/images/osapp4.jpg?width=800px&align=left&classes=border)

<!-- Need a user data link here in point 4.
Update the images in November with proper numbering accounting for the Optional Step 2.
Update the images so that user data field is shown.-->

{{<line_break>}}

#### Measure Inputs
