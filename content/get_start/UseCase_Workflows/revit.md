---
title: "PRM for Revit"
date: 2022-09-21T15:00:28-07:00
weight: 251
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

#### A. Use Case 1: Application of the PRM measure to a user-model created in Revit

This use case showcases how an energy modeler can utilize a BIM (Building Information Modeling) model created in Revit by an architect/engineer and can translate it to a BEM (Building Energy Modeling) model to apply the PRM measure in OpenStudio. The goal is to automatically generate a proposed and one or four baseline models according to [ASHRAE 90.1 PRM rules](/BEM-for-PRM/overview/ashrae) to streamline the energy modeling process. 

[Revit](https://www.autodesk.com/products/revit/overview?term=1-YEAR&tab=subscription) is a building information modeling software used for planning, designing, constructing and managing buildings and infrastructure. The following workflow is based on the capabilities present in [Revit 2024](https://help.autodesk.com/view/RVT/2024/ENU/?guid=GUID-C81929D7-02CB-4BF7-A637-9B98EC9EB38B) at the time of writing and is likely applicable to other versions as well. Changes and updates to the software may lead to modifications in these steps over time. 

The steps to apply the PRM measure to a BIM model created in Revit are as follows:

1. [**Create a model**](#1-create-a-model)
2. [**Define assemblies, internal loads, HVAC**](#2-define-assemblies-internal-loads-hvac)
3. [**Export the model**](#3-export-the-model)
4. [**Apply 90.1 PRM measure**](#4-apply-901-prm-measure)

{{< line_break >}}

##### **1. Create a model**
 A layout of a 3 storey medium office building was created using the Revit elements like walls, roofs, floors, and windows. Spaces were separated as conference rooms, private as well as open office spaces based on their function. 
    
|Plan| 3D Model |
|:-:|:-:|
|![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/revitplan_DDstage.png?width=500px&align=right&classes=border,alignCenter)|![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/revit3D_DDstage.png?width=700px&align=right&classes=border,alignCenter)|


 ##### **2. Define assemblies, internal loads, HVAC**
- Envelope Construction: Envelope assemblies were assigned using schematic construction types present in the Revit built-in library. Generic or actual construction types can also be used based on the amount of information available at hand.  

- Internal Loads: Internal loads were assigned based on the default loads present in Revit. The defaults could be based on the building typology or on space by space method according to various standards and databases (e.g., ASHRAE 90.1, ASHRAE 62.1 and CBECS data). Here, the occupancy, lighting and power definitions, and schedules were assigned to spaces based on space-by-space method according to ASHRAE 90.1-2010/62.1 standards. 

- Thermal Zones: Thermal zones were assigned based on the core/perimeter layout. 

- HVAC: A gas furnace inside a Packaged Air Conditioning Unit (PACU) with Variable Air Volume (VAV) reheat was modeled with one system serving each floor.

![HVAC](/BEM-for-PRM/get_start/UseCase_Workflows/images/DD_HVAC_Revit.png?width=700px&align=right&classes=border,alignCenter)


 ##### **3. Export the model**
Then, the Revit model was exported as an OpenStudio model using [Revit Systems Analysis](https://help.autodesk.com/view/RVT/2024/ENU/?guid=GUID-200338BB-B394-4492-9A11-1A2A80A45AAE) feature since it offers a streamlined workflow and effectively translates most of the gbXML content into BEM using OpenStudio measures. "Rooms and Spaces" mode was used which allows the BEM model to inherit all rooms and incorporate the pre-assigned thermal zoning from the BIM model. It provided a neat model with minor missing pieces such as the plenum area which was corrected manually before running the PRM measure. 

Note that the basic import feature of the OpenStudio application (i.e. the user interface) only translates a limited set of gbXML contents including geometry, constructions, thermal zones, and schedules. The translation of the remaining contents can be achieved using OpenStudio measures.

![Export](/BEM-for-PRM/get_start/UseCase_Workflows/images/image-2023-8-28_21-21-5-5.png?width=500px&align=right&classes=border,alignCenter)

{{% notice info %}}
Other import alternatives could be used such as the [gbXML export](https://help.autodesk.com/view/RVT/2024/ENU/?guid=GUID-586B9574-64DA-47BC-B8EC-DEF2D565928F). Some of the options include generating a gbXML file using Revit native gbXML export, Systems Analysis or Revit Insight. Any information not translated properly during the export process or missing must be addressed before running the PRM measure.
{{% /notice %}}

 ##### **4. Apply 90.1 PRM measure**
Finally, the 90.1 PRM measure was applied in OpenStudio by following the steps mentioned in the sections [Use with OS App](/BEM-for-PRM/get_start/os_app/how_run_measure), [Use with OS SDK using CLI](/BEM-for-PRM/get_start/os_cli/run_the_measure) or [Use with OS SDK API](/BEM-for-PRM/get_start/os_engine/call_use_api).

Key information required to run the PRM measure includes space types for lighting power density, building area type for window to wall ratio, HVAC system selection and water heater system, and process loads. 
Thus, before running the measure, it is important to check the exported BEM model for any missing information and addressing them by following the steps provided in [Best Practices](#best-practices) section to ensure a successful measure run. 

**Once the PRM measure is applied on the BEM model, a proposed as well as one to four baseline models following the set of PRM rules were generated.**

{{<line_break>}}

##### Best Practices 

  - [Standard Space or Building Type Mapping](/BEM-for-PRM/user_guide/add_compliance_data/building_type/user_data_building) 
         
       - Cross validate different [building area types](/BEM-for-PRM/user_guide/add_compliance_data/building_type/user_data_building). In PRM there are three building type categories used to determine: the baseline window-to-wall-ratio (WWR), HVAC system type, and service hot water (SHW) system type that needs to have the same type assigned to them. For example, if the section of a building model is categorized as an "Office (<=5000 ft^2)", it must not be classified as a "Multifamily" building type to determine the baseline model SHW system.
       - If required, manually map space types from Revit to those listed in PRM to align them accurately. 

  - HVAC System Integration 
       
       One of the key data elements often missing in the initial stage of design is related to the HVAC systems. For example, ideal load air system commonly defined for earlier design phases since a complete HVAC system is not typically necessary. Additionally, various export methodologies might not be able to translate the HVAC system design into energy models. Following steps must be used to ensure a smooth PRM run. 

       - Address missing HVAC system data including details like fan power distribution ratios, crucial for a fully automated solution. 
       - A solution that can be adopted is generating the export using Revit Systems Analysis's built-in functionality. This feature generates a complete OpenStudio model using a gbXML based schema that includes a comprehensive HVAC system description, making it ready for the OSSTD-PRM generation process. 

  - Other building service systems 
       - Ensure data related to other building service systems such as SHW, heating, renewable and system controls, is included in the BEM model to avoid inaccuracies in the baseline model. 
       - Consider default settings or templates provided by OpenStudio for creating these systems if necessary. 