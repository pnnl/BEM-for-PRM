---
title: "API documentation"
date: 2022-09-21T15:01:04-07:00
weight: 223
draft: false
pre: "<b>- </b>"
---

The description of PRM API methods can be found in this [API document](/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/).

In addition, the PRM method is a subroutine under the package of [**OpenStudio Standard**](https://github.com/NREL/openstudio-standards) project. Therefore, **OpenStudio Standard** API methods can be accessed from the same routine. For example:

```ruby
require 'openstudio'
require 'openstudio-standard'

standard = Standard.build("90.1-PRM-2019")
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel(model_path + base_model).get

standard.model_remove_prm_hvac(model)
standard.model_remove_prm_ems_objects(model)

thermal_zones = []
model.getThermalZones.each do |zone|
    thermal_zones.append(zone)
  end
end

# Hot water loop
hot_water_loop = standard.model_add_hw_loop(model, 'NaturalGas')
# Heat pump loop
condenser_loop = standard.model_get_or_add_heat_pump_loop(model, 'Electricity',
                                                          'Electricity',
                                                          heat_pump_loop_cooling_type:'CoolingTower')
standard.model_add_water_source_hp(model, thermal_zones, condenser_loop, ventilation: true)
standard.model_add_doas(model, thermal_top_zones,
                        system_name: 'DOAS TOP System',
                        doas_type: 'DOASCV',
                        hot_water_loop: hot_water_loop,
                        chilled_water_loop: nil)
```

The above code loads a model, removes the existing HVAC systems, and calls the **OpenStudio Standard** methods to add a water source heat pump system.

However, using those functions is out of the scope of this documentation. Using those functions shall always refer to the official [**OpenStudio Standard API documentation**](https://www.rubydoc.info/gems/openstudio-standards).

The PRM routine also can be used to work with OpenStudio APIs. Access to the OpenStudio API documentation can be found on the [**OpenStudio official website**](https://s3.amazonaws.com/openstudio-sdk-documentation/index.html).
