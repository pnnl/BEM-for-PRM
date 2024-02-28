---
title: "PRM for Revit"
date: 2022-09-21T15:00:28-07:00
weight: 251
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

#### A. Use Case 1: Application of the PRM measure to a user-model created in Revit

This use case includes translatiing a BIM model created in Revit to a BEM model and applying PRM measure to automatically generate a baseline and a proposed model based on the PRM rules. 

Revit is a building information modeling software used for planning, designing, constructing and managing buildings and infrastructure. The following workflow is based on the capabilities present in Revit 2024 and is likely applicable to other versions as well. 

The steps to apply the PRM measure to a BIM model created in Revit are as follows:

1. [**Create geometry**](#1-create-geometry)
2. [**Define assemblies, internal loads, HVAC**](#2-define-assemblies-internal-loads-hvac)
3. [**Export model: Different routes**](#3-export-model-different-routes)
4. [**Apply 90.1 PRM method**](#4-apply-901-prm-method)

{{< line_break >}}

##### **1. Create a geometry**
 - Create a layout of the building using the Revit elements like walls, roofs, floors, and windows based on the design stage of the model. 
        
    - For a Schematic Design (SD) model, a simple shoe-box model could be used to represent the model, envelope elements can be generated using the Revit conceptual massing feature and the windows could be modeled as a percentage of exterior wall area. 
        
    - For a Design Development (DD) model, consider separating some spaces based on their function. Model walls, roofs, floors an windows using actual Revit elements. 
        
    - For a Construction Document (CD) model, more detailed architectural, structural and lighting layouts should be created along with the actual building elements.
    
|Plan (DD stage)| 3D Model (DD stage) |
|:-:|:-:|
|![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/revitplan_DDstage.png?width=500px&align=right&classes=border,alignCenter)|![userdata](/BEM-for-PRM/get_start/UseCase_Workflows/images/revit3D_DDstage.png?width=700px&align=right&classes=border,alignCenter)|


 ##### **2. Define assemblies, internal loads, HVAC**
- Envelope Construction: Assign envelope assemblies using either: 
    - Generic layer-by-layer assemblies from Revit built-in library (typical for SD phase). 
    - Schematic construction types present in the Revit build-in library (typical for DD phase).
    - Actual construction defined in the Revit model (typical for CD phase).

- Internal Loads: Assign internal loads based on the default loads present in Revit. The defaults could be based on the building typology (preferred for SD phase) or on space by space method (preferred for DD phase) according to various standards and databases (e.g., ASHRAE 90.1, ASHRAE 62.1 and CBECS data). For a CD phase, additionally, the lighting definitions are based on the actual modeled luminaires. 

- Thermal Zones: Assign the thermal zones. It could be single thermal zone for each floor of the building for an SD stage model or based on the core/perimeter layout for DD and CD stage models. A single thermal zone may encompass several spaces. 

- HVAC: Assign an HVAC system. 
    - For an SD model, HVAC system could be skipped since an ideal load air system system automatically generated in the export process. 
    - For DD and CD model, as an example for this workflow, a variable air volume (VAV) reheat packaged rooftop unit can be modeled with one system serving each floor. Any appropriate HVAC system must be modeled. 


 ##### **3. Export the model**
Export the model from Revit (BIM) to OpenStudio (BEM) by any of the following methods. Few differences between the export methods are highlighted below. 
- gbXML export: All gbxml exports include essential model information, such as geometry, construction, internal gain definitions and schedules. There are some differences mentioned for each below: 
   - Revit native gbXML 
   - Revit Systems Analysis gbXML : Additional information included related to HVAC objects. 
   - Autodesk Insight gbXML : Additional information included related to HVAC objects.

{{% notice info %}}
Note that the basic import feature of the OpenStudio application (i.e. the user interface) only translates a limited set of gbXML contents including geometry, constructions, thermal zones, and schedules. The translation of the remaining contents can be achieved using OpenStudio measures.
{{% /notice %}}

- Revit Systems Analysis OpenStudio model 
    - Offers a streamlined workflow 
    - Effectively translates most of the gbXML content into BEM using OpenStudio measures. 
    - Various export modes available like a) Use Conceptual Masses and Building Elements (can be preferred for SD models), b) Use Rooms or Spaces (can be preferred for DD and CD models)

{{% notice info %}}
"Use Rooms or Spaces" allows the BEM model to inherit all rooms and incorporate the pre-assigned thermal zoning from the BIM model. In the BIM model multiple spaces can be associated with the same thermal zone indicating that they are controlled by a single thermostat and this is reflected in the BEM model.
{{% /notice %}}


 ##### **4. Apply 90.1 PRM method**
Apply the 90.1 PRM meausre in OpenStudio by following the steps provided in the initial sections of this user guide based on the method adopted. 

Key information required to run the PRM measure include space types for lighting power density, building area type for window to wall ratio, HVAC system selection and water heater system, and process loads. 
Thus, before running the measure, it is important to check the exported BEM model for any missing information and addressing them by following the steps provided for Model Preparation below to ensure a successful measure run. 

 - Model Preparation 
     - Standard Space Type Mapping 
       
       Mapping of the space types is a common challenge across different export routes and design phases.   
       - Cross validate different building area types. In PRM there are three building type categories used to determine: the baseline window-to-wall-ratio (WWR), HVAC system type, and service hot water (SHW) system type that needs to have the same type assigned to them. For example, if the section of a building model is categorized as an "Office (<=5000 ft^2)", it must not be classified as a "Multifamily" building type to determine the baseline model SHW system.
       - If required, manually map space types from Revit to those listed in PRM to align them accurately. 

     - HVAC System Integration 
       
       One of the key data elements often missing in the initial stage of design is related to the HVAC systems. For example, ideal load air system commonly defined for SD phase since a complete HVAC system is not typically necessary. Additionally, various export methodologies might not be able to translate the HVAC system design into energy models. 

       - Address missing HVAC system data including details like fan power distribution ratios, crucial for a fully automated solution. 
       - A solution that can be adopted is generating the export using Revit Systems Analysis's built-in functionality. This feature generates a complete OpenStudio model using a gbXML based schema that includes a comprehensive HVAC system description, making it ready for the OSSTD-PRM generation process. 

     - Other building service systems 
       - Ensure data related to other building service systems such as SHW, heating, renewable and system controls, is included in the BEM model to avoid inaccuracies in the baseline model. 
       - Consider default settings or templates provided by OpenStudio for creating these systems if necessary. 


**Once the model is prepared and the PRM measure is applied on the BEM model, a baseline as well as a proposed model following the set of PRM rules would be generated.**

{{<line_break>}}

##### **Best Practices**
  - Revit Systems Analysis must be preferred to export Revit models to OpenStudio for a successful application of the PRM measure. 
  - The Design Deveelopment phase, based on its level of detail, might be the most suitable for exporting to OpenStudio and then applying the PRM measure to automate the generation of a baseline and proposed model for code compliance. 
  - The Schematic Design phase model could be avoided since it lacks essential HVAC system data and the generated baseline model is influenced by its simplified zoning assumptions. 
