---
title: "Lights"
date: 2022-09-21T15:06:12-07:00
weight: 3251
draft: false
pre: "<b>- </b>"
---

- [Lights](#lights)
- [has_retail_display_exception](#has_retail_display_exception)
- [has_unregulated_exception](#has_unregulated_exception)
- [unregulated_category](#unregulated_category)


{{< line_break >}}

#### Lights

{{%attachments title="User Data CSV File:" style="orange" pattern=".*\.(csv)$"/%}}

The `user_data_lights.csv` contains compliance data for lights compliance calculation. A sample of data record is shown below.

![user_data_lights](/BEM-for-PRM/user_guide/add_compliance_data/images/user_data_lights_sample.PNG?width=700px&align=left&classes=border,alignLeft)

#### has_retail_display_exception
Set this field to **yes** if the lights object qualifies as display lighting: the lighting must be specifically designed and directed to highlight merchandise and separately controlled from the general lighting. If it doesn't apply, leave this field blank or set to **no**.
#### has_unregulated_exception
Set this field to **yes** if the lights object qualifies as unregulated lighting. If it doesn't apply, leave this field blank or set to **no**. The following lighting system are considered unregulated.
- Display or accent lighting that is an essential element for the function performed in galleries, museums, and - uments.
- Lighting that is integral to equipment or instrumentation and is installed by its manufacturer.
- Lighting specifically designed for use only during medical or dental procedures and lighting integral to - ical equipment.
- Lighting integral to both open and glass-enclosed refrigerator and freezer cases.
- Lighting integral to food warming and food preparation equipment.
- Lighting specifically designed for the life support of nonhuman life forms.
- Lighting in retail display windows, provided the display area is enclosed by ceiling-height partitions.
- Lighting in interior spaces that have been specifically designated as a registered interior historic landmark.
- Lighting that is an integral part of advertising or directional signage.
- Exit signs.
- Lighting that is for sale or lighting educational demonstration systems.
- Lighting for theatrical purposes, including performance, stage, and film and video production.
- Lighting for television broadcasting in sporting activity areas.
- Casino gaming areas.
- Furniture-mounted supplemental task lighting that is controlled by automatic shutoff and complies with - ndard 90.1-2019 Section 9.4.1.3(c).
- Mirror lighting in dressing rooms and accent lighting in religious pulpit and choir areas.
- Parking garage transition lighting—lighting for covered vehicle entrances and exits from buildings and parking structures—that complies with Standard 90.1-2019 Section 9.4.1.2(a) and 9.4.1.2(c); each transition zone shall not exceed a depth of 66 ft inside the structure and a width of 50 ft.
#### unregulated_category
This field is used to inform the code reviewer about which category of unregulated lighting the model represented. It does not affect the baseline building generation. 