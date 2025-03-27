---
title: "Space"
date: 2022-09-21T15:06:18-07:00
weight: 3221
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

#### Introduction

The `userdata_space.csv` file can be used to change the window-to-wall ratio and also provides an alternative way to assign Appendix G lighting space types to the model. See [model_requirements](/BEM-for-PRM/user_guide/model_requirements/standards_space_type) for additional information on lighting space types and the list of valid entries.
The .csv file consists of the following fields.

- [name](#name)
- [building_type_for_wwr](#building_type_for_wwr)
- [num_std_ltg_types](#num_std_ltg_types)
- [std_ltg_type0x](#std_ltg_type0x)
- [std_ltg_type_frac0x](#std_ltg_type_frac0x)
- [has_residential_exception](#has_residential_exception)

The userdata_space.csv file allows the application of multiple lighting space types to a single Space object in the model. Each lighting space type entered into the userdata_space_type.csv file must be paired with a fraction value to indicate the percentage of the floor area for spaces that is to be assigned to that lighting space type.

For example, consider a space named CoreOfficeSpace, that contains 25% private office, 50% open office, and 25% corridor. The entries in the userdata_space.csv file would be as follows:

| name            |    building_type_for_wwr     | num_std_ltg_types | std_ltg_type01              | std_ltg_type_frac01 | std_ltg_type02 | std_ltg_type_frac02 | std_ltg_type03       | std_ltg_type_frac03 | has_residential_exception |
| --------------- | :--------------------------: | :---------------: | --------------------------- | :-----------------: | -------------- | :-----------------: | -------------------- | :-----------------: | :-----------------------: |
| CoreOfficeSpace | Office 5,000 to 50,000 sq ft |         3         | office - enclosed <= 250 sf |        0.25         | office - open  |        0.50         | corridor - all other |        0.25         |           false           |

The resulting baseline will have a weighted average lighting power of the three assigned lighting spacetypes based on the fractions in the table. Spaces not listed in the table will default to the _StandardsSpaceType_ property of the _SpaceType_ object. An error will result if the fractions supplied by the user do not add up to 1.0.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires.

##### **name**

Name of the Space object in the model.

##### **building_type_for_wwr**

Building type for Window to Wall ratio. This field is used to determine the baseline model's window-to-wall ratio. It should be set to one of the following values that best describes the proposed model's building type.

- `All others`
- `Grocery store`
- `Healthcare (outpatient)`
- `Hospital`
- `Hotel/motel <= 75 rooms`
- `Hotel/motel > 75 rooms`
- `Office 5,000 to 50,000 sq ft`
- `Office <= 5,000 sq ft`
- `Office > 50,000 sq ft`
- `Restaurant (full service)`
- `Restaurant (quick service)`
- `Retail (stand alone)`
- `Retail (strip mall)`
- `School (primary)`
- `School (secondary and university)`
- `Warehouse (nonrefrigerated)`

##### **num_std_ltg_types**

Number of lighting space types. If a space is divided into 3 number of lighting space types as shown in the example above, enter 3.

##### **std_ltg_type0x**

Type of standard lighting space where x represents the index/count (1,2,3 and so on). If a space has multiple lighting spaces, mention each of the type separately, for example, std_ltg_type01 = office-enclosed<=250 sf, std_ltg_type02 = office-open etc.

##### **std_ltg_type_frac0x**

Fraction of the floor area for spaces that is to be assigned to that lighting space type.

##### **has_residential_exception**

This allows the modeler to claim an exception for residential/dwelling units where the lighting systems are connected via receptacles and not included in the design documents. If True, LPD will be assigned based on the rules of 90.1 2019 **Table G3.1, Section 6 Lighting**. This is applicable only for proposed models.

- A value of **true** for a given space indicates an exception applies to the space.
- A value of **false** or no value indicates there is no exception.

{{<line_break>}}

A similar capability exists for Space Type objects through the [userdata_space_type.csv](/BEM-for-PRM/user_guide/add_compliance_data/space/user_data_space_type) file.
