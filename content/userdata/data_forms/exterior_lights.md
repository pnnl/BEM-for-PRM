---
title: "Exterior Lights"
date: 2022-07-11T16:29:40-07:00
weight: 7
draft: false
---

User Data CSV File: user_data_exterior_lights.csv

Each exterior lights object in the model can be assigned up to nine exterior lighting subcategories in the user data file based on Appendix G Table G3.6. The lighting subcategories are designated by the labels:  
end_use_subcategory_01  
*through*  
end_use_subcategory_09


For each subcategory, the appropriate quantity must also be listed, as designated by:  
end_use_measurement_value_01  
*through*  
end_use_measurement_value_09

The units of the measurement value are indicated by the last part of the subcategory name:   
'area' = area in square feet   
'perim' = perimiter length in linear feet  
'qty' = integer quantity of items  

Note: the measurement values in the user data table need to be the total quantities for the applicable exterior lights object (accounting for any multipliers). If there is a multiplier in the object, it will be changed to '1' for the baseline model.

Credit can only be taken for exterior lights that are classified as tradeable. Thus, each exterior lighting object in the model must be configured to represent either only tradeable or only non-tradeable subcategories. If any non-tradeable subcategories are included, the entire object will be treated as non-tradeable for the baseline.

In order to work with the Appendix G code database, the following subcategory names must be used in the user data file:

#### Tradeable Exterior Lighting Subcategories
parking_lots_and_drives_area  
walkways_less_than_10_ft_wide_perim  
walkways_10_ft_wide_or_greater_area  
plaza_areas_area  
special_feature_areas_area  
stairways_area  
main_entries_area  
other_doors_perim  
canopies_area  
open_areas_including_vehicle_sales_lots_area  
street_frontage_for_vehicle_sales_perim  

#### Non-Tradeable Exterior Lighting Subcategories
nontradeable_general_qty  
building_facades_area  
building_facades_perim  
automated_teller_machines_per_location_qty  
automated_teller_machines_per_machine_qty  
entries_and_gates_area  
loading_areas_for_emergency_vehicles_area  
drive_through_windows_and_doors_qty  
parking_near_24_hour_entrances_qty  
roadway_parking_qty  



