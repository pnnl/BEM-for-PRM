---
title: "Electric Equipment"
date: 2022-09-21T15:06:12-07:00
weight: 3241
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

#### Introduction 

The `userdata_electric_equipment.csv` contains compliance data for electric equipment compliance calculations that includes receptacles and process loads including motors, elevators and refrigeration equipment loads. 

<!--It allows user to enter  A sample of data record is shown below.-->

<!--![user_data_building]() -->

It consists of the following fields: 

- [name](#name)
- [fraction_of_controlled_receptacles](#fraction_of_controlled_receptacles)
- [receptacle_power_savings](#receptacle_power_savings)
- [motor_horsepower](#motor_horsepower)
- [motor_efficiency](#motor_efficiency)
- [motor_is_exempt](#motor_is_exempt)
- [elevator_weight_of_car](#elevator_weight_of_car)
- [elevator_rated_load](#elevator_rated_load)
- [elevator_counter_weight_of_car](#elevator_counter_weight_of_car)
- [elevator_speed_of_car](#elevator_speed_of_car)
- [elevator_number_of_stories](#elevator_number_of_stories)
- [elevator_number_of_lifts](#elevator_number_of_lifts)
- [elevator_area_ft2](#elevator_area_ft2)
- [elevator_ventilation_cfm](#elevator_ventilation_cfm)
- [refrigeration_equipment_class](#refrigeration_equipment_class)
- [refrigeration_equipment_volume](#refrigeration_equipment_volume)
- [refrigeration_equipment_total_display_area](#refrigeration_equipment_total_display_area)

For example, consider 2 elevators for 3 stories in a building. One of the line items in the .csv would be put in as follows:

|name|fraction_of_controlled_receptacles|receptacle_power_savings|motor_horsepower|motor_efficiency|motor_is_exempt|elevator_weight_of_car|elevator_rated_load|
|:--:|:--------------------------------:|:----------------------:|:--------------:|:--------------:|:------------:|:--------------------:|:-----------------:|
|2 Elevator Lift Motors|||1999|0.72|false|1000|900|

elevator_counter_weight_of_car|elevator_speed_of_car|elevator_number_of_stories|elevator_number_of_lifts|elevator_area_ft2|elevator_ventilation_cfm|refrigeration_equipment_class|
|:----------------------------:|:-------------------:|:------------------------:|:----------------------:|:---------------:|:----------------------:|:---------------------------:|
|1200|200|3|2|25|1000|||

|refrigeration_equipment_volume|refrigeration_equipment_total_display_area|
|:----------------------------:|:----------------------------------------:|
|0|0|

Line items for receptacles and refrigeration equipments can also be added in the same .csv in a similar manner.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires. 

##### **name** 
Name of the electric equipment object in the model. 

##### **fraction_of_controlled_receptacles**
This field is used to input the percentage of all controlled receptacles as a fraction. It requires a float value between 0 and 1. This value will be used to calculate the receptacle power credits according to the methodology mentioned in **Table G3.1 Section 12**. 

##### **receptacle_power_savings**
This field is used to input the receptacle power savings directly instead of entering the fraction of controlled receptacles. One thing to remember is if both fraction_of_controlled_receptacles and receptacle_power_savings are defined in the .csv, the PRM measure will use fraction_of_controlled_receptacles.

##### **motor_horsepower**
This field is used to input the horsepower of the motor. 

##### **motor_efficiency**
This field is used to input the efficiency of the motor as a percentage. It must be set up with a value from **Table G3.9.1**. 

##### **motor_is_exempt**
Set this field to **true** if the motor is exempt. If it doesn't apply, set it as **false** or leave empty. 

##### **elevator_weight_of_car**
This field is used to input the weight of the elevator car in pounds. 

##### **elevator_rated_load**
This field is used to input the proposed design elevator load at which to operate, in pounds.

##### **elevator_counter_weight_of_car**
This field is used to input the counterweight of the elevator car from **Table G3.9.2**, in pounds. 

##### **elevator_speed_of_car**
This field is used to input the speed of the proposed elevator, in feet per minute. 

##### **elevator_number_of_stories**
This field is used to input the number of stories (including basement) that is covered by the elevator. 

##### **elevator_number_of_lifts**
This field is used to input the number of elevators. 

##### **elevator_area_ft2**
This field is used to input the area of the elevator, in square feet. 

##### **elevator_ventilation_cfm**
This field is used to input elevator ventilation fan specification, in Watt per cfm. 

##### **refrigeration_equipment_class**
This field is used to input the class of the refreigeration equipment. it consists of a combination of an equipment family code, and operating mode code and a rating temperature as shown in **Table G3.10.2**. 

##### **refrigeration_equipment_volume**
Ths field is used to input the volume of the chiller or frozen compartment, in cubic feet. Refer to **Table G3.10.1**. 

##### **refrigeration_equipment_total_display_area**
This field is used to input the total display area of the refrigeration equipment. 

{{<line_break>}}