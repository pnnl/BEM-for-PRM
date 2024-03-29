---
title: "Appendix G PRM Rules"
alternative_title: "Regulations" 
date: 2022-09-21T14:48:04-07:00
weight: 11
draft: false
pre: "<b>- </b>"
---

{{< line_break >}}

#### Appendix G Performance Rating Method Rules and OSSTD-PRM

The "Create ASHRAE 90.1-2019 PRM Model" measure provides a detailed implementation of the rules of the Appendix G Performance Rating Method. Below are some of the key changes that are applied by the tool when creating the Baseline and Proposed model: 

##### **Rules for Baseline Model**

- **Apply baseline envelope rules**
  - Space conditioning type
  - Constructions
  - Insulation levels
  - Window-to-wall ratio
  - Skylight-to-roof ratio
  - Infiltration
- **Apply baseline lighting rules**
  - Lighting power
  - Lighting occupancy sensor control
- **Determine and create baseline HVAC systems** 
  - Remove user model HVAC
  - Determine baseline HVAC based on building type and Appendix G rules
  - Create new baseline HVAC systems
  - Apply special rules for computer rooms and laboratories
- **Size HVAC systems**
  - Apply HVAC efficiencies
  - Apply fan power adjustments
  - Apply baseline HVAC controls
- **Rotate building model through cardinal directions**
- **Check for unmet load hours and rectify, if needed**
- **Handle additional compliance data**
  - Multiple building types
  - Plug load measures
  - Elevators
  - Exterior lighting
  - Interior lighting exceptions
  - Number of systems per zone

##### **Rules for Proposed Model**

- **Apply proposed envelope rules**
  - Infiltration
- **Apply proposed lighting rules** 
  - Residential lighting power
- **Remove pipe losses for SWH**
- **Adjust computer room equipment schedules**

