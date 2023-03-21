---
title: "Space type"
date: 2022-09-21T15:06:26-07:00
weight: 3212
draft: false
pre: "<b>- </b>"
---

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv)$"/%}}

The user_data_space_type.csv file provides an alternative way to assign Appendix G lighting space types to the model. See [model_requirements](/BEM-for-PRM/user_guide/model_requirements/standards_space_type) for additional information on lighting space types and the list of valid entries.

The user_data_space_type.csv file allows the application of multiple lighting space types to a single SpaceType object in the model. Each lighting space type entered into the user_data_space_type.csv file must be paired with a fraction value to indicate the percentage of the floor area for spaces that is to be assigned to that lighting space type.

For example, consider a space type named CoreOffice, that contains 25% private office, 50% open office, and 25% corridor. The entries in the user_data_space_type.csv file would be as follows:

|name|num_std_ltg_types|std_ltg_type01|std_ltg_type_frac01|std_ltg_type02|std_ltg_type_frac02|std_ltg_type03|std_ltg_type_frac03|
|----|:---------------:|--------------|:-------------------:|--------------|:-------------------:|--------------|:------------------:|
|CoreOffice| 3 |office - enclosed <= 250 sf|0.25|office - open|0.50|corridor - all other|0.25|

The resulting baseline will have a weighted average lighting power of the three assigned lighting spacetypes based on the fractions in the table. Space types not listed in the table will default to the StandardsSpaceType property of the SpaceType object.

An error will result if the fractions supplied by the user do not add up to 1.0.

A similar capability exists for Space objects through the [user_data_space.csv](/BEM-for-PRM/user_guide/add_compliance_data/user_data_space) file.
