---
title: "Limitations"
date: 2022-09-21T14:48:14-07:00
weight: 13
draft: false
pre: "<b>- </b>"
---

Some features of 90.1-2019 Appendix G that have not yet been implemenented into the baseline automation are as follows:
- Baseline humidification is same as user model in the current tool. In some cases the baseline is required to be changed to adiabatic humidification. This has not been implemented.
- Baseline dehumidification is same as user model in the current tool. For constant volume systems, Appendix G indicates that only 25% of the system reheat energy shall be included in the budget building performance. This is not yet implemented, and must be handled via post-processing or an EMS applied to the Baseline model.
- Baseline transformer efficiencies have not been implemented.
- Where service water heater systems in the user model are comprised of multiple distributed systems, the Appendix G model is required to use a single central system. The conversion to a central system has not yet been implemented.
- For models where design ventilation rates are greater than that required by code, then the rules of Appendix G state that the Baseline ventilation rate shall be based on the building code, and will be lower than the user model. The ability to model this difference has not yet been implemented.
- Some Appendix G requirements for district energy systems have not yet been implemented.
- Efficiency requirements for motors serving miscellaneous electrical equipment have not yet been implemented.
- Efficiency requirements for commercial refrigeration equipment have not yet been implemented.
- The baseline requirement has not yet been implemented where lab exhaust fans must be modeled as constant horsepower, reflecting constant-volume stack discharge with outdoor air bypass.

The rules of the 2016 version of 90.1 Appendix G are very similar to the 2019 version. Some differences are as follows:
- In 2016, if Proposed model has combined space/SWH system, the Baseline shall use separate systems. In 2019, a single central SWH system is to be used. This is similar, but maybe not identical to the 2016 version.
- The 2019 version has rules to allow credit for receptacle control, which are not present in the 2016 version.
- In 2016, the infiltration leakage rate is the same in Proposed and Baseline, at 0.4 cfm/sf at 0.3 in water. In 2019, the Proposed model is specified as 0.6 cfm/sf and the Baseline is at 1.0 cfm/sf at 0.3 in water.
