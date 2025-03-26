+++
title = "User Compliance Data"
date = 2022-09-21T15:04:27-07:00
weight = 32
chapter = true
pre = "<b>2. </b>"
+++

### Chapter 3.2

# User Compliance Data

Compliance data (referred to as user data in previous sections) is a set of `.csv` files with fixed format. The data can be used in the ASHRAE 90.1-2019 PRM method to provide additional information that cannot not be found in an OpenStudio model (e.g., `motor_power`). The compliance data can improve the accuracy of the PRM model.

Now, these set of data can be also arranged in a JSON Schema following the schema set provided in the `.json` files. 

SON Schema is a powerful tool for validating the structure and content of JSON data. It defines the expected format of JSON documents, providing a clear, structured way to ensure that data adheres to a defined set of rules. Unlike the `.csv` files, JSON Schema files are not the ones to fill in data, rather they server as as documentation and guideline to structure the data.

For example, a JSON Schema is defined as:
```javascript
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Product",
  "type": "object",
  "properties": {
    "id": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    },
    "price": {
      "type": "number"
    }
  },
  "required": ["id", "name", "price"]
}
```
Following the data rules in the schema, the user shall provide jsons looks like the one below to meet the data validation requirements.
```javascript
{
  "id": 1,
  "name": "Widget",
  "price": 25.99
}
```

{{%attachments title="Download Compliance Data Set" style="orange" pattern=".*\.(zip)$"/%}}
<!-- update it before the nov release -->

Currently there are 9 `.csv` and `.json` files in the compliance data set. Each of them covers a specific set of ASHRAE 90.1-2019 compliance data to OpenStudio object. This chapter will provide the detailed explaination on each of the files.

<!--
{{%expand "Quick link to each compliance data file user guide:" %}}

- [userdata_airloop_hvac.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_airloop_hvac)
- [userdata_airloop_hvac_doas.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_airloop_hvac_doas)
- [userdata_building.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_building)
- userdata_design_specification_outdoor_air.csv
- userdata_electric_equipment.csv
- [userdata_exterior_lights.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_exterior_lights)
- userdata_gas_equipment.csv
- [userdata_lights.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_lights)
- [userdata_space.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_space/)
- [userdata_spacetype.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_space_type/)
- [userdata_thermal_zone.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_thermal_zone/)
- userdata_wateruse_connections.csv
- userdata_wateruse_equipment.csv
- userdata_wateruse_equipment_definition.csv
- [userdata_zone_hvac.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_zone_hvac)
- userdata_zone_infiltration.csv
  {{% /expand%}}
  --> 
