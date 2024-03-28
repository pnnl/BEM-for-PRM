---
title: "Pollination"
date: 2022-09-21T15:00:28-07:00
weight: 252
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

#### B. Use Case 2: Application of the PRM measure to a BIM model - Pollination Workflow

This use case showcases how an energy modeler can utilize a BIM (Building Information Modeling) model created in a BIM tool by an architect/engineer and can translate it to a BEM (Building Energy Modeling) model through Pollination to apply the PRM measure in OpenStudio. The goal is to automatically generate a proposed and baseline model(s) according to [ASHRAE 90.1 2019 Performance Rating Method (Appendix G)](/BEM-for-PRM/overview/ashrae) to streamline the energy modeling process.

[Pollination](https://www.pollination.cloud/) is a complete building performance simulation platform that is used to generate models, run simulations, and deliver results. It has a [Revit plugin](https://www.pollination.cloud/revit-plugin) that helps extract analytical models directly from the design model to get real-time insight on energy consumption, daylight and thermal comfort. The workflow provided is based on using the Revit Plugin for Pollination as a stand-alone application. Additionally, please note that the steps provided are based on the capabilities present in the software at the time of writing. Changes and updates to the software may lead to modifications in these steps over time.

The steps followed for this use case are as follows:

1. [**Create a user model**](#1-create-a-design-model)
2. [**Define the Construction Set, Program type, HVAC**](#2-define-the-construction-set-program-type-hvac)
3. [**Export the model**](#3-export-the-model)
4. [**Apply 90.1 PRM method**](#4-apply-901-prm-method)

{{< line_break >}}

##### **1. Create a user model**

A layout of a 3 storey medium office building was created using the Revit elements like walls, roofs, floors, and windows. The building is rectangular in shape with a footprint measuring 161 ft by 108 ft, where the longer perimeter sides have a north-south orientation and contains 17,420 ft2 per floor. The window to wall ratio (WWR) was around 32%. Spaces were separated as conference rooms, private as well as open office spaces based on their function.

There are two Geometry Calculation options in Pollination that can be adopted - Extruded Floors and Full Volume. For this workflow, extruded floors option was selected.

{{% notice info %}}
Geometry Calculation setting is used to set the underlying engine that Pollination will use to parse the geometry. Full volume setting refers to Honeybee, while the Extruded Floor uses dragonfly.
{{% /notice %}}

**Extruded Floors** option extrudes the room/space/area boundaries with added apertures.

- Apart from Extruded Floors option for geometry calculation, Detailed option for the window calculation method was selected.
- The model was visualized to make sure there are no issues. To avoid having minor issues after the export such as gaps between floors and adjacency issues that can eventually cause the EnergyPlus simulation to fail, the model was previewed in plane and wireframe view as well and following steps were carried out.
  - The height was adjusted to close any gaps formed in the model due to the default heights.
  - The Align option was used to specify Reference Planes and Model Lines in Revit to pull the room/space boundaries to the nearest plane/line within a specified Align Distance defined in Pollination. This can help clean and fix the export without actually modifying the Revit model.
  - Solve Adjacency was set to Full.

![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/DD_Extr_Step6_After_Fix.png?width=700px&align=right&classes=border,alignCenter)

##### **2. Define the Construction Set, Program type, HVAC**

Details related to "Construction Set", "Program Type" and "HVAC" were specified within Pollination.

- [Construction Sets](https://docs.pollination.cloud/user-manual/revit-plugin/export-analytical-model): Several options for construction templates with different vintages are available, and can also be edited. For this workflow, `2019 Vintage`, `Climate Zone 5` and `Steel Framed` type were selected.

- [Program Types](https://docs.pollination.cloud/user-manual/revit-plugin/export-analytical-model): Several options for load definitions and schedules for people, lighting, equipment, infiltration, ventilation, setpoint, service hot water corresponding to a list of space types and different vintages, are available and can be edited. Here, `2019 Vintage`, with `Medium Office` as the Building Type and three program types - `Conference`, `OpenOffice` and `Closed Office` were selected accordingly for different spaces.

- [HVAC](https://docs.pollination.cloud/user-manual/revit-plugin/export-analytical-model): Only `Ideal air loads` option is available by default and hence was selected (which was modeled as zone equipment). However, an HVAC system is a key element to run the PRM measure, thus, HVAC was revisited after exporting the model to OpenStudio.
  There is also an option in Pollination to import HVAC systems from OpenStudio that could be considered to assign HVAC systems.

- Service Hot Water (SHW): There is an option for SHW but no system to be selected.

- Space Names: To avoid having random IDs assigned to the space names and thermal zones after the export, `Reset Identifier` option was enabled in Pollination's final step before export. This option typically enables Pollination to parse the correct space names into the OpenStudio model.

![construction](/BEM-for-PRM/get_start/UseCase_Workflows/images/Constructionsets_programtype_HVAC.png?width=700px&align=right&classes=border,alignCenter)

##### **3. Export the model**

The model was exported from Pollination by clicking on the export button in the final step (as shown in the image above) as an `.osm` file. It was further imported to OpenStudio and boundary conditions were checked to make sure it imported correctly.

|                                               3D User Model in OpenStudio rendered by surface type                                               |                                                3D User Model in OpenStudio rendered by boundary condition                                                |
| :----------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------: |
| ![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/Pollination_OpenStudio_Model.png?width=500px&align=right&classes=border,alignCenter) | ![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/Pollination_Boundary_Condition_model.png?width=500px&align=right&classes=border,alignCenter) |

##### **4. Apply 90.1 PRM method**

Before applying the 90.1 PRM measure, the exported BEM model was checked for any missing/incorrect information and those were addressed by following the steps below.

- The measure tag for Standard Space Types had to be changed to match the names in the [PRM database](/BEM-for-PRM/user_guide/add_compliance_data/building_type/user_data_building) (Standard Space Types were set to `office - enclosed <= 250 sf`, `conference/meeting/multipurpose` and `office - open` depending on the space function). Although this does not cause a simulation error, the PRM baseline measure will not pick the correct space-related input values (e.g., LPD) if this step is skipped.

- The weather file for the location Denver, Colorado was assigned in OpenStudio.

- A Packaged Rooftop Air Conditioner, one for each floor was assigned along with the thermal zones.

![HVAC](/BEM-for-PRM/get_start/UseCase_Workflows/images/HVAC_UserModel_Revit_Pollination.png?width=300px&align=right&classes=border,alignCenter)

**Once the PRM measure was applied on the BEM model, a proposed as well as one to four baseline models following the set of PRM rules were generated, some key details of which are provided below.**

|                                     3D Baseline Model in OpenStudio rendered by surface type                                      |                                             HVAC Layout for the Baseline Model                                             |
| :-------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------: |
| ![Baseline](/BEM-for-PRM/get_start/UseCase_Workflows/images/Pollination_OpenStudio_Model.png?width=500px&align=right,alignCenter) | ![HVAC](/BEM-for-PRM/get_start/UseCase_Workflows/images/HVAC_Baseline_Revit.png?width=700px&align=right,alignCenter) |

- The generated baseline model had a WWR of 30.95%.
- HVAC system transformed to include 1 Hot Water Loop, 3 systems of Type System 5 based on the thermal zones assigned for each room (as shown in the figure above).
- Opaque assemblies were transformed to the following:

| Category      | Dependent Variable                          | Standard Requirement       | Baseline Value                                    |
| ------------- | ------------------------------------------- | -------------------------- | ------------------------------------------------- |
| Exterior Wall | Climate Zone 5B, nonresidential             | U-0.0.084 (R-11.9)         | PRM Steel Framed Exterior Wall R-11.9             |
| Roof          | Climate Zone 5B, nonresidential             | U-0.063 (R-15.87)          | PRM IEAD Roof R-15.87                             |
| Window        | Climate Zone 5B, nonresidential, 30-40% WWR | U-0.57, SHGC-0.39, VT-0.43 | PRM U 0.57 SHGC 0.39 VT 0.4 Simple Glazing Window |

- Lighting Power Density assigned based on the space types.

| Category | Dependent Variable | Standard Requirement | Baseline Value |
| -------- | ------------------ | -------------------- | -------------- |
| LPD      | Office, Open plan  | 1.10 W/ft2           | 1.10 W/ft2     |
|          | Office, Enclosed   | 1.10 W/ft2           | 1.10 W/ft2     |
|          | Office, Conference | 1.30 W/ft2           | 1.30 W/ft2     |

- _*Dependent Variable: The variables that determine the changes from a user model to a baseline model*_
- _*Standard Requirement: The requirements mentioned in the ASHRAE 90.1 2019 PRM method for a baseline model*_
- _*Baseline Value: The values assigned to the generated baseline model to match the Standard Requirements*_
