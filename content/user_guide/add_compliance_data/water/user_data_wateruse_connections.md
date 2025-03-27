---
title: "Water Connections"
date: 2022-09-21T15:06:12-07:00
weight: 3271
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

#### Introduction

The `userdata_wateruse_connections.csv` contains compliance data for hot water and cold water supply temperature schedules. The schedules are used in the simulation to calculate the water demand loads.

<!--![userdata_exterior_lights](/BEM-for-PRM/user_guide/add_compliance_data/images/user_data_exterior_lighting_sample.PNG?width=1000px&align=left&classes=border,alignLeft)-->

It consists of the following data fields:

- [name](#name)
- [hot_water_supply_temperature_schedule](#hot_water_supply_temperature_schedule)
- [cold_water_supply_temperature_schedule](#hot_water_supply_temperature_schedule)

An example of such data can be found in below. It should be noted that the schedule name provided shall be in the user model.

|  name  | hot_water_supply_temperature_schedule | cold_water_supply_temperature_schedule |
| :----: | :-----------------------------------: | :------------------------------------: |
| Shower |   bathroom_daily_hotwater_schedule    |          water_main_schedule           |

Refer to the section below for more information about each field.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires.

##### **name**

Name of the water use connection object.

##### **hot_water_supply_temperature_schedule**

Name of the hot water supply temperature schedule

##### **cold_water_supply_temperature_schedule**

Name of cold water supply temperature schedule
