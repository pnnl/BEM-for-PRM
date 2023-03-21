---
title: "Zone HVAC"
date: 2022-09-21T15:06:12-07:00
weight: 3231
draft: false
pre: "<b>- </b>"
---

- [Zone HVAC](#zone-hvac)
- [Pressure-drop adjustments](#pressure-drop-adjustments)
  - [has_fan_power_credit_fully_ducted](#has_fan_power_credit_fully_ducted)
  - [has_fan_power_credit_return_or_exhaust_flow_control](#has_fan_power_credit_return_or_exhaust_flow_control)
  - [fan_power_credit_exhaust_treatment](#fan_power_credit_exhaust_treatment)
  - [has_fan_power_credit_filtration_m9to12](#has_fan_power_credit_filtration_m9to12)
  - [has_fan_power_credit_filtration_m13to15](#has_fan_power_credit_filtration_m13to15)
  - [clean_filter_pressure_drop_for_fan_power_credit_filtration_m16plus](#clean_filter_pressure_drop_for_fan_power_credit_filtration_m16plus)
  - [fan_power_credit_gas_phase_cleaners](#fan_power_credit_gas_phase_cleaners)
  - [fan_power_credit_biosafety](#fan_power_credit_biosafety)
  - [fan_power_credit_other_than_coil_runaround](#fan_power_credit_other_than_coil_runaround)
  - [has_fan_power_credit_coil_runaround](#has_fan_power_credit_coil_runaround)
  - [fan_power_credit_evaporative_humidifier_or_cooler](#fan_power_credit_evaporative_humidifier_or_cooler)
  - [has_fan_power_credit_sound_attenuation](#has_fan_power_credit_sound_attenuation)
  - [has_fan_power_credit_exhaust_serving_fume_hoods](#has_fan_power_credit_exhaust_serving_fume_hoods)
  - [has_fan_power_credit_lab_or_vivarium_highrise_vertical_duct](#has_fan_power_credit_lab_or_vivarium_highrise_vertical_duct)

{{< line_break >}}

#### Zone HVAC

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv)$"/%}}

The `user_data_zone_hvac.csv` contains compliance data for zone HVAC compliance calculation. A sample of data record is shown below.

![user_data_zone_hvac](/BEM-for-PRM/user_guide/add_compliance_data/images/user_data_zone_hvac_sample.PNG?width=1000px&align=left&classes=border,alignLeft)

#### Pressure-drop adjustments
The following fields can be used to indicate what pressure-drop adjustment devices are present in each zone HVAC object in the proposed model. Each field corresponds to one of the pressure-drop adjustment categories from **Table 6.5.3.1-2**.
##### has_fan_power_credit_fully_ducted
Set this field to the number of return or exhaust systems required by code or accreditation standards to be fully ducted, or systems required to maintain air pressure differentials between adjacent rooms that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_return_or_exhaust_flow_control
Set this field to the number of return and/or exhaust airflow control devices that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### fan_power_credit_exhaust_treatment
Set this field to the sum of the pressure-drop (calculated at fan system design conditions) of all exhaust filters, scrubbers, or other exhaust treatment devices that the zone HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_filtration_m9to12
Set this field to the number of MERV 9 through 12 filters that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_filtration_m13to15
Set this field to the number of MERV 13 through 15 filters that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### clean_filter_pressure_drop_for_fan_power_credit_filtration_m16plus
Set this field to the sum of the pressure-drop (calculated at 2 times the clean filter pressure-drop at fan system design conditions) of all MERV 16+ filters and electronically enhanced filters that the zone HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
##### fan_power_credit_gas_phase_cleaners
Set this field to the sum of carbon and other gas-phase air cleaners' pressure-drop (at the fan system design conditions) that the zone HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
##### fan_power_credit_biosafety
Set this field to the sum of the pressure-drop (calculated at fan system design conditions) of biosafety cabinets that the zone HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
##### fan_power_credit_other_than_coil_runaround
Set this field to the number of energy recovery devices other than coil runaround loop that the zone HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_coil_runaround
Set this field to the number of runaround loop that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### fan_power_credit_evaporative_humidifier_or_cooler
Set this field to the sum of the pressure-drop (calculated at fan system design conditions) of the evaporative humidifier or cooler that the zone HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_sound_attenuation
Set this field to the number of sound attenuation sections of fans serving spaces with design background noise goals below NC35 that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_exhaust_serving_fume_hoods
Set this field to the number of exhaust systems serving fume hoods that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
##### has_fan_power_credit_lab_or_vivarium_highrise_vertical_duct
Set this field to the number of laboratory and vivarium exhaust systems in high-rise buildings that the zone HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.