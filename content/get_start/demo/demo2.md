---
title: "Demo 2"
date: 2022-08-16T13:04:42-07:00
weight: 243
draft: false
pre: "<b>- </b>"
---

### Demo 2: User data

Demo 1 shows the basic functionality of the PRM method. In Demo 2, we will demonstrate how to hanle multiple building types in a model.
It is common for a building to have multiple types, for example, the first floor of the medium office can function as retail spaces and the rest of floors are offices.
In this case, we will need to provide more information for the PRM function so it can properly handle the multi-building types scenario.

{{< line_break >}}

#### User data

To do that, we will need to use the [userdata_space.csv](../../../user_guide/add_compliance_data/user_data_space/). In the `demo2` zip file, there is a `user_data` folder. Inside this folder, we have a `userdata_space.csv` file. Let's open the file to inspect.

![userdata](/BEM-for-PRM/get_start/os_engine/images/demo2_space_userdata.PNG?width=600px&align=right&classes=border,alignLeft)

In the file, we can see all perimeter zones on the first floor are set to `Retail (strip mall)` WWR building type. For lighting space type, the `Perimeter_bot_ZN_2` is set to `retail mall concourse` and the other spaces are set to `retail - whole building`. With this file, we will expect the first floor perimeter spaces have different window to wall ratios and lighting power densities than the other spaces in the building.

{{< line_break >}}

#### Demo 2 results

Before running the same script, we need to update the directories, specifically, the `demo_folder` and `proposed_model`

```Ruby
# directories - change these two variables
demo_folder = 'demo2'
proposed_model = 'geometry_demo_model.osm'

```

![Geometry demo 2](/BEM-for-PRM/get_start/os_engine/images/demo2_geometry_screenshot.png?width=400px&align=right&classes=border,alignLeft)
Immediately, we noticed the WWR on the first loor is much smaller than the WWR on the second and third floor. This is because the ASHRAE 90.1 PRM requires the `Retail (strip mall)` WWR to be `19%`, which is much smaller than the `Office 5,000 to 50,000 sq ft` (`31%`).

| Category      | Apply to                       | Dependent Variable                         | Standard Requirement       | Baseline Value                                    |
| ------------- | ------------------------------ | ------------------------------------------ | -------------------------- | ------------------------------------------------- |
| Exterior Wall | All                            | Climate Zone 4, nonresidential             | U-0.124 (R-8.06)           | PRM Steel Framed Exetrior Wall R-8.06             |
| Roof          | All                            | Climate Zone 4, nonresidential             | U-0.063 (R-15.87)          | PRM IEAD Roof R-15.87                             |
| Window        | All                            | Climate Zone 4, nonresidential, 30-40% WWR | U-0.57, SHGC-0.39, Vt-0.43 | PRM U 0.57 SHGC 0.39 VT 0.4 Simple Glazing Window |
| LPD           | Spaces excluded from user data | Office, whole building                     | 1.10 W/ft2                 | 11.8 W/m2                                         |
| LPD           | First floor 1, 3, and 4        | User data: retail - whole building         | 1.50 W/ft2                 | 16.1 W/m2                                         |
| LPD           | First floor 2                  | User data: retail mall concourse           | 1.70 W/ft2                 | 18.3 W/m2                                         |

**In the next demo, we will show a HVAC transformation example.**

{{< line_break >}}