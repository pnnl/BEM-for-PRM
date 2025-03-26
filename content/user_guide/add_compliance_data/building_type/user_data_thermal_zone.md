---
title: "Thermal Zone"
date: 2022-09-21T15:06:12-07:00
weight: 3222
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

#### Introduction 

The `userdata_thermal_zone.csv` contains compliance data for thermal zone calculations. A thermal zone is typically a space or a collection of spaces that have similar space conditioning requirements. The .csv file consists of the following fields. 

- [name](#name)
- [building_type_for_hvac](#building_type_for_hvac)
- [number_of_systems](#number_of_systems)
- [dcv_exception_thermal_zone](#dcv_exception_thermal_zone)
- [has_health_safety_night_cycle_exception](#has_health_safety_night_cycle_exception)

The userdata_thermal_zone.csv file allows the application of multiple systems in a single thermal zone object in the model. 

For example, consider a thermal zone named Thermal Zone - Retail that consists of shops, restaurants and a large atrium having 3 different HVAC systems. The entries in the userdata_thermal_zone.csv file would be as follows: 

|name|building_type_for_hvac|number_of_systems|dcv_exception_thermal_zone|has_health_safety_night_cycle_exception|
|----|:--------------------:|:---------------:|:------------------------:|:-------------------------------------:|
|Thermal Zone - Retail|retail| 3 |false|false|

Refer to the section below for more information about each field. 

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires. 

##### **name** 
Name of the thermal zone object in the model.

##### **building_type_for_hvac**
This is the building type designation used for determining the Appendix G baseline system type. Valid entries are:
- `residential`
- `public assembly`
- `heated-only storage`
- `retail`
- `other nonresidential`
- `unconditioned`
- `hospital`

##### **number_of_systems**
This allows multiple systems to be modeled conceptually in a single thermal zone. This could be used when a single thermal zone in the model is used to represent multiple thermal zones in the actual building. It could also be used for large zones that contain multiple packaged HVAC systems in the actual building design. When the baseline cooling or heating efficiency depends on system capacity, the capacity determined in the baseline sizing run is divided by number_of_systems before the efficiency lookup is performed.

##### **dcv_exception_thermal_zone**
This allows the modeler to claim an exception to demand controlled ventilation requirements based on the rules of 90.1 **Section 6.4.3.8**. If the exception is taken, then DCV will not be included in the baseline, regardless of the outdoor air flow and occupant density.  
- A value of **true** for a given zone indicates an exception applies to the zone.  
- A value of **false** or no value indicates there is no exception.

##### **has_health_safety_night_cycle_exception**
Appendix G has a general requirement that fans should operate in a cycling mode during unoccupied hours (night cycle). An exception is allowed for spaces that have health or safety mandated minimum ventilation requirements during unoccupied hours.  
- A value of **true** for a given zone indicates an exception applies to the zone.  
- A value of **false** or no value indicates there is no exception.

{{<line_break>}}