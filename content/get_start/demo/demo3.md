---
title: "Demo 3"
date: 2022-08-16T13:04:42-07:00
weight: 244
draft: false
pre: "<b>- </b>"
---

### Demo 3: HVAC

In the last demo, we have a medium office equipped with water source heat pumps for heating and cooling and a DOAS for ventilation.
Also, the **Core Top Zone** is being identified as a high equipment density (`40 W/m2`) room.

According to the ASHRAE 90.1 PRM method, the baseline system type for the medium office shall be a packaged VAV with reheat (system 5) and the **Core Top Zone** shall be separately conditioned by a packaged single zone air conditioner (system 3) according to the exception c.
![HVAC demo 3](/BEM-for-PRM/get_start/os_engine/images/demo3_hvac_systems.png?width=700px&align=right&classes=border,alignLeft)

{{< line_break >}}

#### Demo 3 results

First step, update the directories to `demo3`. It should note the `proposed_model` is changed to `hvac_demo_model.osm`.

```Ruby
# directories - change these two variables
demo_folder = 'demo3'
proposed_model = 'hvac_demo_model.osm'

```

After the simulation run, the baseline HVAC systems shall be the same as the graphic below.
![HVAC baseline demo 3](/BEM-for-PRM/get_start/os_engine/images/demo3_baseline_hvac_system.png?width=700px&align=right&classes=border,alignLeft)

{{< line_break >}}
