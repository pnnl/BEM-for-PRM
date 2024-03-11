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

<!-- Need a user data link here in point 4.
Update the images in November with proper numbering accounting for the Optional Step 2.
Update the images so that user data field is shown.-->

{{<line_break>}}

#### Measure Inputs

![Openstudio download measure](/BEM-for-PRM/get_start/os_app/images/osapp4.jpg?width=800px&align=left&classes=border)

##### **User input 1: Default Building Area Type for Window To Wall Ratio**

Select a default building type for WWR assignment based on 90.1 Appendix G table G 3.1.1-1. The full list of available building area types can be found in the [Baseline generation API, Argument 5](../../../user_guide/prm_api_ref/baseline_generation_api/#argument-5-default_wwr_bldg_type).

##### **User input 2: Default Building Area Type for Service Water Heating**

Select a default building type for SWH system type assignment based on 90.1 Appendix G Table G3.1.1-2. The full list of available building area types can be found in the [Baseline generation API, Argument 6](../../../user_guide/prm_api_ref/baseline_generation_api/#argument-6-default_swh_bldg_type)

##### **User input 3: Default Type for HVAC**

Select a default building type for HVAC system type assignment based on 90.1 Appendix G Table G3.1.1-3. The full list of available building area types can be found in the [Baseline generation API, Argument 4](../../../user_guide/prm_api_ref/baseline_generation_api/#argument-4-default_hvac_bldg_type)

##### **User input 4: Climate Zone**

Select a climate zone to run the PRM measure. Note: PRM cannot select a climate zone from the provided model. This feature is under development.

##### **User input 5: Exempt From Rotations**

Select `TRUE` if the building in has rating authority approval that orientation is dictated by site consideration. Note: [userdata_building](../../../user_guide/add_compliance_data/building_type/user_data_building) can override this selection.

##### **User input 6: Exempt From Unmet Load Hours Check**

Select `TRUE` if the building in has rating authority approval allowing the models to exceed the specified values in Appendix G.

##### **User input 7: Use User Data**

Select `TRUE` if [user data CSVs](../../../user_guide/add_compliance_data) were provided and need to be incorporated in the PRM generation process.

##### **User input 8: User Data Path**

Required if select `TRUE` in **Use User Data**. A valid path string needs to be provided in this input field. The path string shall be the root folder that host all the user data CSV files.

{{% notice warning %}}
For **OpenStudio SDK 3.7** and **OpenStudio Application 1.7**, the string needs to be provided with forward slash. This also applies to Window OS.
Example: `C:\Documents\user_data` needs to be revised to `C:/Documents/user_data`. Failed to compliant with the path format will cause PRM ignores the user data csvs.
{{% /notice %}}

##### **User input 9: Use PRM evaluation package**

Select `TRUE` if the PRM should be run with a customized OpenStudio Standards Package. This can be the source code package downloaded from [OSSTD repo](https://github.com/NREL/openstudio-standards/tree/AppendixG_Dev).

##### **User input 10: Evaluation Package Path**

Required if select `TRUE` in **Use PRM evaluation package**. A valid path string needs to be provided in this input field. The path string shall be pointing to the root folder of the customized OSSTD source code.
