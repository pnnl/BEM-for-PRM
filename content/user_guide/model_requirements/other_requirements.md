---
title: "Other requirements"
date: 2022-09-21T15:05:48-07:00
weight: 313
draft: false
pre: "<b>- </b>"
---

The user model should have occupancy control of lighting incorporated into the lighing use schedules where it is required by ASHRAE 90.1-2019. The program will back-calculate the uncontrolled lighting schedules for the Baseline model based on the occupancy sensor reduction values in Appendix G Table G3.7.

The user model needs to have a fully defined HVAC system. Most of the HVAC objects are deleted when the Appendix G Baseline is created, but some information is taken from the user model and transferred to the Baselne. This includes the following:

* Zone ventilation rates. For zones that require demand control ventilation in the baseline model, ventilation must be provided in terms of both flow per area and flow per person. The program will issue a warning if this is not done.
* Fan schedules
* Proportion of fan power for supply, return, and relief fans
