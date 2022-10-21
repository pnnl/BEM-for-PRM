+++
title = "User compliance data"
date = 2022-09-21T15:04:27-07:00
weight = 32
chapter = true
pre = "<b>2. </b>"
+++

### Chapter 3.2

# User Compliance Data

Compliance data is a set of `.csv` files with fixed format. The data can be used in the ASHRAE 90.1-2019 PRM method to provide additional information that cannot not be found in an OpenStudio model (e.g., `motor_power`). The compliance data can improve the accuracy of the PRM model.

{{%attachments title="Download Compliance Data Set" style="orange" pattern=".*\.(zip)$"/%}}

Currently there are 16 `.csv` files in the compliance data set. Each of them covers a specific set of ASHRAE 90.1-2019 compliance data to OpenStudio object. This chapter will provide the detail explaination on each of the file.

{{%expand "Quick link to each compliance data file user guide:" %}}

- userdata_airloop_hvac.csv
- userdata_airloop_hvac_doas.csv
- userdata_building.csv
- userdata_design_specification_outdoor_air.csv
- userdata_electric_equipment.csv
- [userdata_exterior_lights.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_exterior_lights)
- userdata_gas_equipment.csv
- userdata_lights.csv
- userdata_space.csv
- userdata_spacetype.csv
- userdata_thermal_zone.csv
- userdata_wateruse_connections.csv
- userdata_wateruse_equipment.csv
- userdata_wateruse_equipment_definition.csv
- userdata_zone_hvac.csv
- userdata_zone_infiltration.csv
  {{% /expand%}}
