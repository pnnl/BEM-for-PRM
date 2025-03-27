---
title: "Outdoor Air"
date: 2022-09-21T15:06:12-07:00
weight: 3245
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

### Introduction 

The `userdata_design_specification_outdoor_air.csv` contains compliance data for outdoor air compliance calculation. It allows users to specify alternative Outdoor Air specifications for the baseline model. The proposed model will be the same as the user model. 

The .csv consists of the following data fields.

- [name](#name)
- [outdoor_airflow_per_person](#outdoor_airflow_per_person)
- [outdoor_airflow_per_floor_area](#outdoor_airflow_per_floor_area)
- [outdoor_air_flowrate](#outdoor_air_flowrate)
- [outdoor_air_flow_air_changes_per_hour](#outdoor_air_flow_air_changes_per_hour)

For example, consider a zone named Office WholeBuilding - Md Office Ventilation with 0.0004 cfm per square feet. The entries in the userdata_design_specification_outdoor_air.csv file would be as follows:

|name|outdoor_airflow_per_person|outdoor_airflow_per_floor_area|outdoor_air_flowrate|outdoor_air_flow_air_changes_per_hour|
|:--:|:------------------------:|:----------------------------:|:------------------:|:-----------------------------------:|
|Office WholeBuilding - Md Office Ventilation|0|0.0004|0|0|

Refer to the section below for more information about each field.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires. 

##### **name**

Name of the Design Specification Outdoor Air object in the model. 

##### **outdoor_airflow_per_person**

This field is used to determine the design outdoor air volume flow rate per person, in cfm/person.

##### **outdoor_airflow_per_floor_area**

This field is used to determine the design outdoor air volume flow rate per square meter of floor area, in cfm/ft2.

##### **outdoor_air_flowrate**

This field is used to determine the design outdoor air flow rate, in cfm.

##### **outdoor_air_flow_air_changes_per_hour**

This field is used to determine the design outdoor air volume flow air changes per hour. It is a factor.

{{<line_break>}}

