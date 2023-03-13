---
title: "Limitations"
date: 2022-09-21T14:48:14-07:00
weight: 14
draft: false
pre: "<b> </b>"
---

### Current Limitations of the OSSTD-PRM

{{< line_break >}}

Some features of 90.1-2019 Appendix G that have not yet been implemenented into the baseline automation are summarized below.

#### PRM Rules and Differences
| System      | 90.1 Appendix G version                         | Baseline Model       | Proposed Model                                    |
| ------------- | --------------------- | -------------------------- | ------------------------------------------------- |
| Space and SWH system | 2016             | Separate System    | Combined System            |
|           | 2019             | Single Central SWH System         | Combined System                            |
| Receptacle Control        | 2016 | No credit |  |
|            | 2019                     | Allows credit                |                                          |
| Infiltration leakage rate | 2016             | 0.4 cfm/sf at 0.3 in water    |  0.4 cfm/sf at 0.3 in water            |
|           | 2019             | 1.0 cfm/sf at 0.3 in water         | 0.6 cfm/sf in water |

{{< line_break >}}

#### Features
| System      | Item                        | Baseline Model vs. Proposed Model       | Notes                                    |
| ------------- | --------------------- | -------------------------- | ------------------------------------------------- |
| HVAC components | Humidification             |Same as proposed model in the tool. <br> In some cases, the baseline is required to be changed to adiabatic humidification. | This has not been implemented            |
|           |    Humidification          | For constant volume systems, Appendix G indicates that only 25% of the system reheat energy shall be included in the budget building performance         | This is not yet implemented. <br> It must be handled via post-processing, or an EMS applied to the Baseline model.|
| | SWH             |Where service water heater systems in the user model are comprised of multiple distributed systems, the Appendix G model is required to use a single central system.| The conversion to a central system has not yet been implemented.           |
| | Design Ventilation Rates             |For models where design ventilation rates are greater than that required by code, then the rules of Appendix G state that the Baseline ventilation rate shall be based on the building code and will be lower than the user model.| The ability to model this difference has not yet been implemented.       |
| |DES            |               | Some Appendix G requirements for district energy systems have not yet been implemented.     |
| |Lab exhaust fans            |               |The baseline requirement has not yet been implemented where lab exhaust fans must be modeled as constant horsepower, reflecting constant-volume stack discharge with outdoor air bypass.      |
|Equipment|Transformer Efficiencies         |               | Baseline transformer efficiencies have not been implemented.     |
||Motor Efficiencies         |               |  Efficiency requirements for motors serving miscellaneous electrical equipment have not yet been implemented.    |
||Commercial refrigeration equipment         |               | Efficiency requirements for commercial refrigeration equipment have not yet been implemented.     |
