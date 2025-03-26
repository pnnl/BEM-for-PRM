---
title: "Building"
date: 2022-09-21T15:06:12-07:00
weight: 3221
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv|json)$"/%}}

{{< line_break >}}

#### Introduction 

The `userdata_building.csv` contains compliance data for building compliance calculations. It can be used to define Appendix G HVAC systems, service water heating systems and the window to wall ratio for the entire building. The .csv consists of the following fields. 

<!--![userdata_building](/BEM-for-PRM/user_guide/add_compliance_data/images/user_data_building_sample.PNG?width=400px&align=left&classes=border,alignLeft) -->


- [name](#name)
- [building_type_for_hvac](#building_type_for_hvac)
- [building_type_for_swh](#building_type_for_swh)
- [building_type_for_wwr](#building_type_for_wwr)
- [is_exempt_from_rotations](#is_exempt_from_rotations)

For example, consider a building named Small Office less than 5000 sqft with less than 3 floors and in climate zone 2. The entries in the userdata_building.csv file would be as follows:

|name|building_type_for_hvac|building_type_for_swh|building_type_for_wwr|is_exempt_from_rotations|
|:--:|:--------------------:|:-------------------:|:-------------------:|:----------------------:|
|Small Office| other nonresidential |Office|Office <= 5,000 sq ft|false|

The resulting baseline will have a System 4 - PSZ-HP, electric resistance storage water heater and a 19% wwr. 

{{<line_break>}}

#### Data Fields Details

Here is a detailed explanation of what each data field requires. 

##### **name** 
Name of the building type object in the model. 

##### **building_type_for_hvac**
This field is used to determine the baseline model's HVAC system. It can be overwritten at the zone level (see the documentation for `userdata_thermal_zone.csv`). It should be set to one of the following values that best describes the proposed model's building type.
- `heated-only storage`
- `hospital`
- `other nonresidential`
- `public assembly`
- `residential`
- `retail`
- `unconditioned`

##### **building_type_for_swh**
This field is used to determine the baseline model's service water heating system. It should be set to one of the following values that best describes the proposed model's building type.
- `All others`
- `Automotive facility`
- `Convenience store`
- `Convention center`
- `Courthouse`
- `Dining: Bar lounge/leisure`
- `Dining: Cafeteria/fast food`
- `Dining: Family`
- `Dormitory`
- `Exercise center`
- `Fire station`
- `Grocery store`
- `Gymnasium`
- `Health-care clinic`
- `Hospital and outpatient surgery center`
- `Hotel`
- `Library`
- `Manufacturing facility`
- `Motel`
- `Motion picture theater`
- `Multifamily`
- `Museum`
- `Office`
- `Parking garage`
- `Penitentiary`
- `Performing arts theater`
- `Police station`
- `Post office`
- `Religious facility`
- `Retail`
- `School/university`
- `Sports arena`
- `Town hall`
- `Transportation`
- `Warehouse`
- `Workshop`

##### **building_type_for_wwr**
This field is used to determine the baseline model's window-to-wall ratio. It should be set to one of the following values that best described the proposed model's building type.
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

##### **is_exempt_from_rotations**
Set this field to **true** if it can be demonstrated to the satisfaction of the rating authority that the building orientation is dictated by site considerations. If it doesn't apply, leave this field blank or set to **false**.

{{<line_break>}}