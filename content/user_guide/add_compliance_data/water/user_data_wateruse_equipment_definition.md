---
title: "Water Equip Definition"
date: 2022-09-21T15:06:12-07:00
weight: 3273
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

#### Introduction

The `userdata_wateruse_equipment_definition.csv` contains compliance data the wateruse equipment definition. The object can be found in [Openstudio SDK](https://s3.amazonaws.com/openstudio-sdk-documentation/cpp/OpenStudio-3.7.0-doc/model/html/classopenstudio_1_1model_1_1_water_use_equipment_definition.html). The data file let's user to modify data for an existing wateruse equipment definition object.

It consists of the following data fields:

- [name](#name)
- [peak_flow_rate](#peak_flow_rate)
- [flow_rate_fraction_schedule](#flow_rate_fraction_schedule)
- [target_temperature_schedule](#target_temperature_schedule)

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
