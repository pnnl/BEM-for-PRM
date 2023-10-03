---
title: "Call measure via API"
date: 2022-09-21T15:00:28-07:00
weight: 232
draft: false
pre: "<b>- </b>"
---

This instruction will give a quick overview of the API method that generates baseline model(s) from a provided openstudio model.

- [Create stable baseline building API](#create-stable-baseline-building-api)
- [User data](#user-data)
- [Baseline outputs](#baseline-outputs)

{{< line_break >}}

#### Create stable baseline building API

The ASHRAE 90.1-2019 PRM automation can be called in a Ruby script via its API method. The API method provides great flexibility for creating customized modeling workflow. For example:

```ruby
require `openstudio`
require `openstudio-standard`

model_path = 'C:\demo\output\\'
save_path = 'C:\demo\source\\'
base_model = 'test_model.osm'
modified_model = 'modified_model_demo.osm'
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel(model_path + base_model).get

# Adjust window to wall ratio to 60%
model.getSurfaces.each do |ss|
  unless ss.subSurfaces.empty?
    orig_construction = ss.subSurfaces[0].construction.get
    ss.subSurfaces.sort.each(&:remove)
    new_window = ss.setWindowToWallRatio(0.6, 0.8, true).get
    new_window.setConstruction(orig_construction)
  end
end

# Add overhangs on south facade.
model.getSubSurfaces.each do |s|
  absoluteAzimuth = OpenStudio.convert(s.azimuth, 'rad', 'deg').get + s.space.get.directionofRelativeNorth + model.getBuilding.northAxis
  absoluteAzimuth -= 360.0 until absoluteAzimuth < 360.0
  next if !((absoluteAzimuth >= 135.0) && (absoluteAzimuth < 225.0))
  new_overhang = s.addOverhangByProjectionFactor(0.8, 0)
end

# Build the standard PRM routine
standard = Standard.build('90.1-PRM-2019')
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
hvac_bldg_type = 'other nonresidential'
wwr_bldg_type = 'Office <= 5,000 sq ft'
swh_bldg_type = 'Office'

standard.model_create_prm_stable_baseline_building(model, climate_zone, hvac_bldg_type, wwr_bldg_type, swh_bldg_type, baseline_dir, unmet_load_hours_check=true, debug=false)
```

What the script above did was:

> 1.  Load an OSM model - it could be any model created as a proposed model for creating a baseline system.
> 2.  Make adjustments to the model.
>     - Change the window-to-wall ratio to 60% (adding a span glazing on every wall).
>     - Add overhangs to the south facade of the model.
> 3.  Generate the baseline model by calling the API method.

At the end of the script, the `standard` object calls a function `model_create_prm_stable_baseline_building` is the API function that generates the baseline model.

```ruby
standard.model_create_prm_stable_baseline_building(model, climate_zone, hvac_bldg_type, wwr_bldg_type, swh_bldg_type, baseline_dir, unmet_load_hours_check=true, debug=false)
```

A detail description of the arguments can be found in the [API document](/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/).

{{< line_break >}}

#### User data

In PRM routines, one additional step allows users to add code compliance-specific data to the baseline generation workflow. Data missing in most building energy models, such as space type for a space, lighting space type for space lighting, or motor horsepower for electric equipment, can be added to the user data before calling the API.
The use of the user data can be found in the [Add compliance data](/BEM-for-PRM/user_guide/add_compliance_data/) section.

```ruby
# Create PRM object
standard = Standard.build('90.1-PRM-2019')
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
# Converts user data from .csv file to .json file
json_path = standard.convert_userdata_csv_to_json("#{File.dirname(Dir.pwd)}/user_data", "#{baseline_dir}")
# Load the user data json files into the PRM routine.
standard.load_userdata_to_standards_database(json_path)
# Generate baseline
standard.model_create_prm_stable_baseline_building(model, climate_zone, hvac_bldg_type, wwr_bldg_type, swh_bldg_type, baseline_dir, unmet_load_hours_check=true, debug=false)
```

The above script first creates the PRM object from OpenStudio Standards, then converts the data from `.csv` to `.json` and loads them into the object before generating the baseline. It is important to convert the CSV to JSON before loading them to the PRM object. The [API documents](/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/#convert_userdata_csv_to_json) gives a more detail description of the function `convert_userdata_csv_to_json` and `load_userdata_to_standards_database`.

{{< line_break >}}

#### Baseline outputs

A successful baseline generation creates several baseline model(s) and folders under the `baseline_dir` as shown in the image below.
![Folder structure](/BEM-for-PRM/get_start/os_engine/images/baseline_generation_completion_folder.PNG?width=650px&align=left&classes=border,alignLeft)

**SR\_[NUMBER]**:
Several folders are generated and saved in the `baseline_dir`. They typically start with `SR_` and ends with a number or `PROP`. These folders contain the energy model and their sizing run outputs. The number records the index of the sizing run. `PROP` indicates a sizing run on a proposed model. A more detailed description of the baseline generation steps is described in the [API document](/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/).

**user_data_json**:
The `user_data_json` saves all user-defined data in JSON format. The list includes the complete set of user data regardless there is data or not.

**final.osm**: A baseline OpenStudio model at 0 rotation degrees.

**final_NUM.idf**:
Depending on whether the baseline model case meets the G3.1(5).a building rotation requirements, the baseline generation produces one or four EnergyPlus files. If there are no rotations, the folder would only contain one `final.idf`; else, there would be`final_0.idf`, `final_90.idf`, `final_180.idf`, and `final_270.idf`.
