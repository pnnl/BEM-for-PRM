---
title: "Other requirements"
date: 2022-09-21T15:05:48-07:00
weight: 313
draft: false
pre: "<b>- </b>"
---

The user model should have occupancy control of lighting incorporated into the lighing use schedules where it is required by ASHRAE 90.1-2019. The program will back-calculate the uncontrolled lighting schedules for the Baseline model based on the occupancy sensor reduction values in Appendix G Table G3.7.

The user model needs to have a fully defined HVAC system. Most of the HVAC objects are deleted when the Appendix G Baseline is created, but some information is taken from the user model and transferred to the Baselne. This includes the following:

- Zone ventilation rates. For zones that require demand control ventilation in the baseline model, ventilation must be provided in terms of both flow per area and flow per person. The program will issue a warning if this is not done.
- Fan schedules
- Proportion of fan power for supply, return, and relief fans

The proposed model is generated from the user model with following changes:

1. Update lighting power density for hotel/motel guest rooms (Table 9.6.1) and dwelling units (0.6 W/ft2) in multi-family. Alternate calculation is not available yet and will be released in the future.
2. Reset the air leakage rate to 0.6 cfm/ft2 at pressure differential of 0.3 in. of water.
3. Reset the computer room equipment schedule to the prescribed schedule
4. Remove service hot water system pipe losses

The OSSTD-PRM will perform an annual simulation using the proposed model to extract the unmet load hours. The unmet load hours shall be less than 300 hours as specified by the PRM method.
