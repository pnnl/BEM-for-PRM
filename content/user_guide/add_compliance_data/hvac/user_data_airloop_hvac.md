---
title: "Airloop HVAC"
date: 2022-09-21T15:06:12-07:00
weight: 3243
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{< line_break >}}

#### Introduction 

The `userdata_airloop_hvac.csv` contains compliance data for airloop HVAC compliance calculation. It allows the user to specify pressure drop adjustments, air side economizer design considerations, exhaust air energy recovery design considerations as well as dcv exceptions.  

<!--![user_data_airloop_hvac](/BEM-for-PRM/user_guide/add_compliance_data/images/user_data_airloop_hvac_sample.PNG?width=1200px&align=left&classes=border,alignLeft)-->

It consists of the following data fields: 
- [name](#name)
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
- [Air-side economizer](#air-side-economizer)
  - [economizer_exception_for_gas_phase_air_cleaning](#economizer_exception_for_gas_phase_air_cleaning)
  - [economizer_exception_for_open_refrigerated_cases](#economizer_exception_for_open_refrigerated_cases)
- [Exhaust air energy recovery](#exhaust-air-energy-recovery)
  - [exhaust_energy_recovery_exception_for_toxic_fumes_etc](#exhaust_energy_recovery_exception_for_toxic_fumes_etc)
  - [exhaust_energy_recovery_exception_for_type1_kitchen_hoods](#exhaust_energy_recovery_exception_for_type1_kitchen_hoods)
  - [exhaust_energy_recovery_exception_for_type_distributed_exhaust](#exhaust_energy_recovery_exception_for_type_distributed_exhaust)
  - [exhaust_energy_recovery_exception_for_dehumidifcation_with_series_cooling_recovery](#exhaust_energy_recovery_exception_for_dehumidifcation_with_series_cooling_recovery)
- [dcv_exception_airloop](#dcv_exception_airloop)

An example of sample data is shown here:

|name|has_fan_ power_credit _fully_ducted|has_fan_power_ credit_return_or_ exhaust_flow_control|fan_power_credit_ exhaust_treatment|has_fan_power_ credit_filtration_m9to12|has_fan_power_ credit_filtration_m13to15|clean_filter_pressure_ drop_for_fan_power_ credit_filtration_m16plus|
|:--:|:-------------------------------:|:-------------------------------------------------:|:--------------------------------:|:------------------------------------:|:-------------------------------------:|:----------------------------------------------------------------:|
|Perimeter_ZN _1 ZN PSZ-AC-2|0|0|0|0|0|yes|0|

fan_power_credit_ gas_phase_cleaners|fan_power_credit_ biosafety|fan_power_credit_ other_than_coil_ runaround|fan_power_credit_ evaporative_humidifier_or_ cooler|has_fan_power_ credit_sound_ attenuation|has_fan_power_ credit_exhaust_ serving_fume_hoods|
|:--------------------------------:|:------------------------:|:----------------------------------------:|:-----------------------------------------------:|:------------------------------------:|:---------------------------------------------:|
|0|0|0|0|0|0|0|0|

|has_fan_power_ credit_lab_or_ vivarium_highrise_ vertical_duct|economizer_exception_for_ gas_phase_air_cleaning|economizer_exception_for_ open_refrigerated_cases|exhaust_energy_recovery_ exception_for_toxic_ fumes_etc|exhaust_energy_recovery_ exception_for_type1_ kitchen_hoods|exhaust_energy_recovery_ exception_for_type_ distributed_exhaust|exhaust_energy_recovery_ exception_for_dehumidifcation_ with_series_cooling_ recovery|dcv_exception_ airloop|
|:---------------------------------------------------------:|:---------------------------------------------:|:----------------------------------------------:|:---------------------------------------------------:|:-------------------------------------------------------:|:------------------------------------------------------------:|:--------------------------------------------------------------------------------:|:-------------------:|
|0|false|false|false|false|false|false|false|

Refer to the section below for more information about each field.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires. 

##### **name**
Name of the Air Loop HVAC object in the model. 

##### **Pressure-drop adjustments**
The following fields can be used to indicate what pressure-drop adjustment devices are present in each airloop HVAC object in the proposed model. Each field corresponds to one of the pressure-drop adjustment categories from **Table 6.5.3.1-2**.

- ##### has_fan_power_credit_fully_ducted
  Set this field to the number of return or exhaust systems required by code or accreditation standards to be fully ducted, or systems required to maintain air pressure differentials between adjacent rooms that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_return_or_exhaust_flow_control
  Set this field to the number of return and/or exhaust airflow control devices that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### fan_power_credit_exhaust_treatment
  Set this field to the sum of the pressure-drop (calculated at fan system design conditions) of all exhaust filters, scrubbers, or other exhaust treatment devices that the airloop HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_filtration_m9to12
  Set this field to the number of MERV 9 through 12 filters that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_filtration_m13to15
  Set this field to the number of MERV 13 through 15 filters that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### clean_filter_pressure_drop_for_fan_power_credit_filtration_m16plus
  Set this field to the sum of the pressure-drop (calculated at 2 times the clean filter pressure-drop at fan system design conditions) of all MERV 16+ filters and electronically enhanced filters that the airloop HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
- ##### fan_power_credit_gas_phase_cleaners
  Set this field to the sum of carbon and other gas-phase air cleaners' pressure-drop (at the fan system design conditions) that the airloop HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
- ##### fan_power_credit_biosafety
  Set this field to the sum of the pressure-drop (calculated at fan system design conditions) of biosafety cabinets that the airloop HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
- ##### fan_power_credit_other_than_coil_runaround
  Set this field to the number of energy recovery devices other than coil runaround loop that the airloop HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_coil_runaround
  Set this field to the number of runaround loop that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### fan_power_credit_evaporative_humidifier_or_cooler
  Set this field to the sum of the pressure-drop (calculated at fan system design conditions) of the evaporative humidifier or cooler that the airloop HVAC object includes. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_sound_attenuation
  Set this field to the number of sound attenuation sections of fans serving spaces with design background noise goals below NC35 that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_exhaust_serving_fume_hoods
  Set this field to the number of exhaust systems serving fume hoods that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.
- ##### has_fan_power_credit_lab_or_vivarium_highrise_vertical_duct
  Set this field to the number of laboratory and vivarium exhaust systems in high-rise buildings that the airloop HVAC object includes. If the number is 1, the field can also be set to **yes**. If it doesn't apply, leave this field blank or set to 0.

##### **Air-side economizer**
The following fields can be used to indicate special design consideration regarding air-side economizer that cannot be inferred from the proposed model. These are used by the software to determine if an airloop HVAC object complies with the certain exceptions of **Section G3.1.2.6**.
- ##### economizer_exception_for_gas_phase_air_cleaning
  Set this field to **true** if the airloop HVAC object include gas-phase air cleaning to meet the requirements of **Standard 62.1, Section 6.1.2**. If it doesn't apply leave this field blank or set to **false**.
- ##### economizer_exception_for_open_refrigerated_cases
  Set this field to **true** where the use of outdoor air for cooling by the airloop HVAC object would affect supermarket open refrigerated case-work systems. If it doesn't apply leave this field blank or set to **false**.

##### **Exhaust air energy recovery**
  The following fields can be used to indicate special design consideration regarding air energy recovery that cannot be inferred from the proposed model. These are used by the software to determine if an airloop HVAC object complies with the certain exceptions of **Section G3.1.2.10**.
- ##### exhaust_energy_recovery_exception_for_toxic_fumes_etc
  Set this field to **true** if the airloop HVAC object is exhausting toxic, flammable, or corrosive fumes or paint or dust. If it doesn't apply leave this field blank or set to **false**.
- ##### exhaust_energy_recovery_exception_for_type1_kitchen_hoods
  Set this field to **true** if the airloop HVAC object includes commercial kitchen hoods (grease) classified as Type 1 by NFPA 96. If it doesn't apply leave this field blank or set to **false**.
- ##### exhaust_energy_recovery_exception_for_type_distributed_exhaust
  Set this field to **true** when the airloop HVAC object's largest exhaust source is less than 75% of the design outdoor airflow. If it doesn't apply leave this field blank or set to **false**.
- ##### exhaust_energy_recovery_exception_for_dehumidifcation_with_series_cooling_recovery
  Set this field to **true** when the airloop HVAC object is requiring dehumidification that employ energy recovery in series with the cooling coil. If it doesn't apply leave this field blank or set to **false**.

##### **dcv_exception_airloop**
   Set this field to **true** if any of the exceptions under the section 6.4.3.8 applies. If it doesn't apply leave this field blank or set to **false**.

{{<line_break>}}