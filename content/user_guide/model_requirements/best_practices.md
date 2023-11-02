---
title: "Best Practices"
date: 2022-09-21T15:05:48-07:00
weight: 311
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

#### Here are some best practices to run the PRM measure: 

- **The space types should be defined in the model** : The lighting space type is used to determine the baseline lighting power density and lighting control savings. Read more about it in [**Standards space type**](/BEM-for-PRM/user_guide/model_requirements/standards_space_type/) section. 
If the User Compliance Data files contain lighting space type assignments, there is a hierachy for determining the final assignments:

     1. user_data_space is first priority
     2. user_data_space_type is second priority
     3. StandardsSpaceType property of the SpaceType objest is lowest priority

- **Dates in the schedule must have a "MM/DD" formatting** : Where is this inserted? Any other formatting such as "31 Dec" will cause the measure to fail. 

- **Defined Outdoor Flowrates** : The model must have the [**design_specification_outdoor air**](/BEM-for-PRM/user_guide/add_compliance_data/hvac/user_data_design_specification_outdoor_air/) input defined (e.g., outdoor air flow per person) for ventilation requirements since OSSTD PRM measure used ASHRAE 62.1. 

- **The user model needs to have a fully defined HVAC system** : Most of the HVAC objects are deleted when the Appendix G Baseline is created, but some information is taken from the user model and transferred to the Baselne. This includes the following:

     - Zone ventilation rates. For zones that require demand control ventilation in the baseline model, ventilation must be provided in terms of both flow per area and flow per person. The program will issue a warning if this is not done.
     - Fan schedules
     - Proportion of fan power for supply, return, and relief fans       

  The proposed model should reflect actual systems design (systems such as Ideal Air Load should be avoided).

- **Include occupancy control of lighting** : The user model should have occupancy control of lighting incorporated into the lighing use schedules where it is required by ASHRAE 90.1-2019. The program will back-calculate the uncontrolled lighting schedules for the Baseline model based on the occupancy sensor reduction values in Appendix G Table G3.7.

