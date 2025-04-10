---
title: "Exterior Lights"
date: 2022-07-11T16:29:40-07:00
weight: 3262
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{< line_break >}}

#### Introduction 

The `userdata_exterior_lights.csv` contains compliance data for exterior lighting compliance calculation. It allows the user to specify the sub categories for all the exterior lights along with the appropriate measurement units. 

<!--![userdata_exterior_lights](/BEM-for-PRM/user_guide/add_compliance_data/images/user_data_exterior_lighting_sample.PNG?width=1000px&align=left&classes=border,alignLeft)-->

It consists of the following data fields:

- [name](#name)
- [num_ext_lights_subcats](#num_ext_lights_subcats)
- [end_use_subcategory](#end_use_subcategory)
  - [Tradeable Exterior Lighting Subcategories](#tradeable-exterior-lighting-subcategories)
  - [Non-Tradeable Exterior Lighting Subcategories](#non-tradeable-exterior-lighting-subcategories)
- [end_use_measurement_value](#end_use_measurement_value)

For example, consider a building with an parking area that has 2 different kinds of exterior lights, one for the parking lots and drive areas and another for the exterior building facade area facing the parking lot. The entries in the userdata_exterior_lights.csv file would be as follows:

|name|num_ext_lights_subcats|end_use_subcategory_01|end_use_measurement_value_01|end_use_subcategory_02|end_use_measurement_value_02|
|:--:|:--------------------:|:--------------------:|:--------------------------:|:--------------------:|:--------------------------:|
|Parking Lights|2|parking_lots_and_drives_area|100|building_facades_area|150|

Refer to the section below for more information about each field.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires. 

##### **name** 
Name of the exterior lights object. 

##### **num_ext_lights_subcats**
Number of subcategories for exterior lights. 

##### **end_use_subcategory**

Each exterior lights object in the model can be assigned up to nine exterior lighting subcategories in the user data file based on **Appendix G Table G3.6**. The lighting subcategories are designated by the labels:  
_end_use_subcategory_01_ through _end_use_subcategory_09_

In order to work with the **Appendix G** code database, the following subcategory names **must** be used in the user data file:

- Tradeable Exterior Lighting Subcategories

>- `parking_lots_and_drives_area`  
>- `walkways_less_than_10_ft_wide_perim`  
>- `walkways_10_ft_wide_or_greater_area`  
>- `plaza_areas_area`  
>- `special_feature_areas_area`  
>- `stairways_area`  
>- `main_entries_area`  
>- `other_doors_perim`  
>- `canopies_area`  
>- `open_areas_including_vehicle_sales_lots_area`  
>- `street_frontage_for_vehicle_sales_perim`

- Non-Tradeable Exterior Lighting Subcategories

>- `nontradeable_general_qty`  
>- `building_facades_area`  
>- `building_facades_perim`  
>- `automated_teller_machines_per_location_qty`  
>- `automated_teller_machines_per_machine_qty`  
>- `entries_and_gates_area`  
>- `loading_areas_for_emergency_vehicles_area`  
>- `drive_through_windows_and_doors_qty`  
>- `parking_near_24_hour_entrances_qty`  
>- `roadway_parking_qty`

{{% notice warning %}}
It is critical to use the identical string in the above list. Failure to match the list could result erroneous data in the PRM model.
{{% /notice %}}

{{< line_break >}}

##### **end_use_measurement_value**

For each subcategory, the appropriate quantity must also be listed, as designated by:  
_end_use_measurement_value_01_ through _end_use_measurement_value_09_

The units of the _measurement value_ are indicated by the last part of the subcategory name:  
`area` = area in square feet  
`perim` = perimeter length in linear feet  
`qty` = integer quantity of items

{{% notice tip %}}
Note: the measurement values in the user data table need to be the total quantities for the applicable exterior lights object (accounting for any multipliers). If there is a multiplier in the object, it will be changed to '1' for the baseline model.
{{% /notice %}}

Credit can only be taken for exterior lights that are classified as tradeable. Thus, each exterior lighting object in the model must be configured to represent either only tradeable or only non-tradeable subcategories. If any non-tradeable subcategories are included, the entire object will be treated as non-tradeable for the baseline.

{{<line_break>}}