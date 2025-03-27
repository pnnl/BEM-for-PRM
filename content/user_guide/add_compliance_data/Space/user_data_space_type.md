---
title: "Space type"
date: 2022-09-21T15:06:26-07:00
weight: 3222
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{<line_break>}}

#### Introduction

The `userdata_space_type.csv` file provides an alternative way to assign Appendix G lighting space types to the model. See [model_requirements](/BEM-for-PRM/user_guide/model_requirements/standards_space_type) for additional information on lighting space types and the list of valid entries. The .csv file consists of the following fields.

- [name](#name)
- [num_std_ltg_types](#num_std_ltg_types)
- [std_ltg_type0x](#std_ltg_type0x)
- [std_ltg_type_frac0x](#std_ltg_type_frac0x)

The userdata_space_type.csv file allows the application of multiple lighting space types to a single SpaceType object in the model. Each lighting space type entered into the userdata_space_type.csv file must be paired with a fraction value to indicate the percentage of the floor area for spaces that is to be assigned to that lighting space type.

For example, consider a space type named CoreOffice, that contains 25% private office, 50% open office, and 25% corridor. The entries in the user_data_space_type.csv file would be as follows:

| name       | num_std_ltg_types | std_ltg_type01              | std_ltg_type_frac01 | std_ltg_type02 | std_ltg_type_frac02 | std_ltg_type03       | std_ltg_type_frac03 |
| ---------- | :---------------: | --------------------------- | :-----------------: | -------------- | :-----------------: | -------------------- | :-----------------: |
| CoreOffice |         3         | office - enclosed <= 250 sf |        0.25         | office - open  |        0.50         | corridor - all other |        0.25         |

The resulting baseline will have a weighted average lighting power of the three assigned lighting spacetypes based on the fractions in the table. Space types not listed in the table will default to the StandardsSpaceType property of the SpaceType object.

An error will result if the fractions supplied by the user do not add up to 1.0.

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires.

##### **name**

Name of the space type object in the model.

##### **num_std_ltg_types**

Number of lighting space types. If a space is divided into 3 number of lighting space types as shown in the example above, enter 3.

##### **std_ltg_type0x**

Type of standard lighting space where x represents the index/count (1,2,3 and so on). If a space has multiple lighting spaces, mention each of the type separately, for example, std_ltg_type01 = office-enclosed<=250 sf, std_ltg_type02 = office-open etc.

##### **std_ltg_type_frac0x**

Fraction of the floor area for spaces that is to be assigned to that lighting space type.

{{<line_break>}}

A similar capability exists for Space objects through the [user_data_space.csv](/BEM-for-PRM/user_guide/add_compliance_data/space/user_data_space) file.
