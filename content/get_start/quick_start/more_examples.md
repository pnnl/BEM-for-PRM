---
title: "More examples"
date: 2022-08-19T09:36:35-07:00
weight: 233
draft: false
pre: "<b>- </b>"
---

This section will walk through an additional script that alters our prototype model before running the baseline.

OpenStudio Standard package is a powerful package that allows user to manipulate their models with simple commands - particularly high-level HVAC templates and model data modifications.

In this section, we will use the OpenStudio Standard package to:

1. Reset window to wall ratio to 60%
2. Add overhangs to the south facade
3. Replace the HVAC system with DOAS + water source heat pumps.

![modified_model](/BEM-for-PRM/get_start/quick_start/image/modified_model_image.PNG?width=800px&classes=border)

First, let's load the model and standard package.

```ruby
model_path = "#{File.dirname(Dir.pwd)}/output/"
save_path = "#{File.dirname(Dir.pwd)}/source/"
base_model = 'test_model.osm'
modified_model = 'modified_model_demo.osm'

# Load standard package
standard = Standard.build('90.1-PRM-2019')

# load model
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel(model_path + base_model).get
```

After the model is loaded, we can firstly modify the window-to-wall ratio using a simple function: `setWindowToWallRatio` from OpenStudio API.

```ruby
# Remove HVAC system and EMS (if any)
standard.model_remove_prm_hvac(model)
standard.model_remove_prm_ems_objects(model)
# remove transformers
model.getElectricLoadCenterTransformers.each(&:remove)

# Loop through every sub surface
model.getSurfaces.each do |ss|
  unless ss.subSurfaces.empty?
    orig_construction = ss.subSurfaces[0].construction.get
    ss.subSurfaces.sort.each(&:remove)
    # Set WWR to 60% with sill height of 0.8 m.
    new_window = ss.setWindowToWallRatio(0.6, 0.8, true).get
    new_window.setConstruction(orig_construction)
  end
end
```

Next, we will add overhangs to the south facade

```ruby
# loop sub surfaces
model.getSubSurfaces.each do |s|
    # get the absolute azimuth of the sub surface
    absoluteAzimuth = OpenStudio.convert(s.azimuth, 'rad', 'deg').get + s.space.get.directionofRelativeNorth + model.getBuilding.northAxis
    # filter out non-south sub surfaces
    next if !((absoluteAzimuth >= 135.0) && (absoluteAzimuth < 225.0>))
    # Adds overhangs 0.8 m
    s.addOverhangByProjectionFactor(0.8, 0)
end
```

Last, we will add a DOAS + water source heat pump to the model. We will add one DOAS per floor and one heat pump per thermal zone.

```ruby
# split thermal zones by floors, exclude plenum zones
thermal_top_zones = []
thermal_mid_zones = []
thermal_bot_zones = []
model.getThermalZones.each do |zone|
  unless /Plenum/ =~ zone.name.get
    if /top/ =~ zone.name.get
      thermal_top_zones.append(zone)
    elsif /bot/ =~ zone.name.get
      thermal_bot_zones.append(zone)
    else
      thermal_mid_zones.append(zone)
    end
  end
end

# Add a hot water plant loop for DOAS
hot_water_loop = standard.model_add_hw_loop(model, 'NaturalGas')

# Add a heat pump plant loop
condenser_loop = standard.model_get_or_add_heat_pump_loop(model, 'Electricity',
                                                          'Electricity',
                                                          heat_pump_loop_cooling_type: 'CoolingTower')

# Top floor zone
standard.model_add_water_source_hp(model, thermal_top_zones, condenser_loop, ventilation: true)
standard.model_add_doas(model, thermal_top_zones,
                        system_name: 'DOAS TOP System',
                        doas_type: 'DOASCV',
                        hot_water_loop: hot_water_loop,
                        chilled_water_loop: nil)

# Mid floor zone
standard.model_add_water_source_hp(model, thermal_mid_zones, condenser_loop, ventilation: true)
standard.model_add_doas(model, thermal_mid_zones,
                        system_name: 'DOAS MID System',
                        doas_type: 'DOASCV',
                        hot_water_loop: hot_water_loop,
                        chilled_water_loop: nil)

# Bottom floor zone
standard.model_add_water_source_hp(model, thermal_bot_zones, condenser_loop, ventilation: true)
standard.model_add_doas(model, thermal_bot_zones,
                        system_name: 'DOAS BOT System',
                        doas_type: 'DOASCV',
                        hot_water_loop: hot_water_loop,
                        chilled_water_loop: nil)
```

That's it. In the end, we will call the baseline function to generate a baseline.

```ruby
bldg_type = 'MediumOffice'
climate_zone = 'ASHRAE 169-2013-4A'
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
default_hvac_bldg_type = 'other nonresidential'
default_wwr_bldg_type = 'Office 5,000 to 50,000 sq ft'
default_swh_bldg_type = 'Office'

baseline_models = standard.model_create_prm_stable_baseline_building(model, bldg_type, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, nil, baseline_dir, run_all_orients=false, unmet_load_hours_check=true, debug=false)
```

![before_after_compare](/BEM-for-PRM/get_start/quick_start/image/before_after_comparison.PNG?width=600px&classes=border)

Now we can have a more visual comparison between the prototype and baseline. In the baseline, the windows are shrunk, and the overhangs are removed. Also, the HVAC system was changed from DOAS + water source heat pump to Packaged VAV system (system type 5).
