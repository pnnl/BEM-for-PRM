---
title: "Window API"
date: 2022-09-21T15:06:59-07:00
weight: 332
draft: false
pre: "<b> </b>"
---

- [Quick use](#quick-use)
- [Function workflow](#function-workflow)
- [How to use](#how-to-use)
- [Input arguments](#input-arguments)
  - [user_model](#user_model)
  - [climate_zone](#climate_zone)
- [Output values](#output-values)
  - [result](#result)
  - [wwr_info](#wwr_info)
- [Multi building area type handling](#multi-building-area-type-handling)

{{< line_break >}}

#### Quick Use

```ruby
wwr_building_type = "Office <= 5,000 sq ft"
standard = Standard.build("90.1-PRM-2019")
result, wwr_info = standard.model_apply_prm_baseline_window_to_wall_ratio(model, climate_zone='', wwr_building_type: wwr_building_type)
```

The above code adjusts the window-to-wall ratio in the model to the specified building type. Each building type maps to a window-to-wall ratio.

| Building Area Types                 | Baseline building vertical fenestration percentage of gross-above-grade wall area |
| ----------------------------------- | --------------------------------------------------------------------------------- |
| `Grocery store`                     | 7%                                                                                |
| `Warehouse (nonrefrigerated)`       | 6%                                                                                |
| `School (secondary and university)` | 22%                                                                               |
| `School (primary)`                  | 22%                                                                               |
| `Retail (strip mall)`               | 20%                                                                               |
| `Retail (stand alone)`              | 11%                                                                               |
| `Restaurant (quick service)`        | 34%                                                                               |
| `Restaurant (full serivce)`         | 24%                                                                               |
| `Office > 50,000 sq ft`             | 40%                                                                               |
| `Office <= 5,000 sq ft`             | 19%                                                                               |
| `Office 5,000 to 50,000 sq ft`      | 31%                                                                               |
| `Hotel/motel > 75 rooms`            | 34%                                                                               |
| `Hotel/motel <= 75 rooms`           | 24%                                                                               |
| `Hospital`                          | 27%                                                                               |
| `Healthcare (outpatient)`           | 21%                                                                               |
| `All other`                         | 40%                                                                               |

The mapping is shown in the above table. Once the function identifies the correct building area, it will apply the window-to-wall ratio to the entire model.

{{< line_break >}}

#### Function workflow

{{<mermaid align="center">}}
graph LR;
A(Model) -->|Loop| B(Spaces)
B -->|Apply building area type| C(BAT)
B -->|Surface conditioning category| D{Condition}
D -->|Conditioned space| E(Surfaces)
C -->|Target WWR| F(Adjust Ratio)
E -->|Existing WWR| F(Adjust WWR)
{{</mermaid>}}

{{< line_break >}}

#### How to use

As the workflow diagram indicated, the function checks each zone and surface to determine its conditioned type. The conditioned type of a zone is determined after model sizing simulation. Therefore, it is critical to note that this function will only work after a successful full-year simulation or sizing run. To do that, follow the script below before calling the function.

```ruby
# Initiate the standard to PRM and load the model (model needs to be v3.4 or lower)
standard = Standard.build("90.1-PRM-2019")
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel(model_path).get

# Make a copy of the model
user_model = BTAP::FileIO.deep_copy(model)
# Run annual simulation
standard.model_run_simulation_and_log_errors(user_model, run_dir = "#{prototype_dir}/PROP")
# Modify the window to wall ratio.
result, wwr_info = standard.model_apply_prm_baseline_window_to_wall_ratio(user_model, climate_zone, wwr_building_type: wwr_building_type)
```

{{% notice warning %}}
If the model is not simulated, this function can still run. However, it will not apply a target WWR to the model because it cannot find any conditioned surface to adjust WWR.
{{% /notice %}}

{{< line_break >}}

#### Function behavior

It is essential to understand the function behaviors before calling this function.

1. If the target WWR is smaller than the model overall WWR, the function will proportionally reduce the size of every window while keeping their center coordinates fixed
2. If the target WWR is greater than the model overall WWR, the function will:
   - remove all windows in surfaces and add new ones to meet the required WWR. This step will only apply to surfaces that have windows
   - Plenums (including air loop supply and return plenums) are not considered in the WWR expanding algorithm.
   - If surfaces with windows reach a 90% WWR, the total gross WWR of the building is still smaller than the target WWR. The function will create new windows on every other surface to meet the target WWR for the building. The size of new windows should be proportional to their host surface.
     ![WWR_EXPAND](/BEM-for-PRM/user_guide/prm_api_ref/images/wwr_expand_test.PNG?width=800px&align=left&classes=border)
   - If there are doors on the surface, the maximum WWR for the surface will readjust to ensure the total fenestration area and doors are 90% of the surface area.
     ![WWR_DOOR_EXPAND](/BEM-for-PRM/user_guide/prm_api_ref/images/wwr_expand_doors_test.PNG?width=800px&align=left&classes=border)

{{< line_break >}}

#### Input arguments

##### user_model

`user_model` shall be the OpenStudio model object (`OpenStudio::Model::Model`) that contains a complete description of a building energy model in `.osm` format.

##### climate_zone

`climate_zone` is a String data. However, it is no longer used in the ASHRAE 90.1 2019 PRM method, so it is OK to type in an empty string.

##### wwr_building_type

`wwr_building_type` is a hash key, and the value shall be user-defined. The default is `nil`. The `wwr_building_type` shall be a string taken from the list below:

- `Warehouse (nonrefrigerated)`
- `School (secondary and university)`
- `School (primary)`
- `Retail (strip mall)`
- `Retail (stand alone)`
- `Restaurant (quick service)`
- `Restaurant (full serivce)`
- `Office > 50,000 sq ft`
- `Office <= 5,000 sq ft`
- `Office 5,000 to 50,000 sq ft`
- `Hotel/motel > 75 rooms`
- `Hotel/motel <= 75 rooms`
- `Hospital`
- `Healthcare (outpatient)`
- `Grocery store`
- `All other`

{{% notice info %}}
It is important to keep the `wwr_building_type` string identical to the list above. Otherwise, the function will set the target building `WWR` to `40%`.
{{% /notice %}}

{{< line_break >}}

#### Output values

##### result

The result is a boolean. `true` indicates a successful generation and `false` otherwise.

##### wwr_info

The `wwr_info` is a hash map that maps the `wwr_building_type` to its correspondent target `wwr`, which is applied when altering the model window-to-wall ratio.

```
{"Office <= 5,000 sq ft"=>19.0}
```

{{< line_break >}}

#### Multi building area type handling

One feature of the PRM routine is the capability of handling multiple building area types in one energy model.
For example, if an energy model is simulating a building mixed with retail (strip mall) and offices (>50,000 sq ft). The baseline vertical fenestration percentage requirements differ for these two building area types. Previously, modelers had to manually adjust the generated baseline model to meet the compliance request. In the new workflow, we introduce the user data to handle this situation in the baseline generation runtime.

##### Create user data

To do that, we need to create user data. The example we are using is a typical small office model. In this model, we will identify one space as a retail (strip mall) space and the rest are an office (>50,000 sq ft).

In this example, we will edit the [userdata_space](/BEM-for-PRM/user_guide/add_compliance_data/user_data_space/) file in a spreadsheet.

![wwr_user_data](/BEM-for-PRM/user_guide/prm_api_ref/images/wwr_user_data_space.PNG)

{{% notice info %}}
It is recommended to edit the file in a spreadsheet cause this can avoid any special characters in the string misidentified by the OSSTD-PRM.
In this case, `Office > 50,000 sq ft` can be mis-identified into two columns as `Office > 50` and `000 sq ft`.
{{% /notice %}}

Once the user data is set, save the file as a `.csv` into a `user_data` folder.

##### Set up the script

Let's first set up basic data and load the user energy model

```ruby
# Set up directories
model_dir = "#{File.dirname(Dir.pwd)}/output"
user_model_dir = "#{File.dirname(Dir.pwd)}/source"
user_model_name = 'proposed_model.osm'
user_data_path = "#{File.dirname(Dir.pwd)}/user_data"

# Set up basic parameters
climate_zone = "ASHRAE 169-2013-4A"
wwr_building_type = "Office <= 5,000 sq ft"

# load user model
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel("#{user_model_dir}/#{user_model_name}").get
user_model = BTAP::FileIO.deep_copy(model)

# Initialize the standard routine
standard = Standard.build("90.1-PRM-2019")
```

Now we have everything we need, let's load the user data

```ruby
# Convert the .csv files into json files and save the json files into model_dir
json_path = standard.convert_userdata_csv_to_json(user_data_path, model_dir)
# load the user data into standard object instance
standard.load_userdata_to_standards_database(json_path)
# Add the user data to the user model.
standard.handle_multi_building_area_types(user_model, climate_zone, '', wwr_building_type, '', {})
```

The three lines of code above accomplished three goals:

1. Convert the .csv files into JSON files and save the JSON files into `model_dir`
2. Load the user data into a Standard object instance
3. Call the Standard instance to add user data to the user energy model.

The above three steps are crucial to the overall process. The function `handle_multi_building_area_types()` has six arguments. However, in this example, we only focus on the WWR API function. So supplying `user_model`, `climate_zone`, and `wwr_building_type` and setting the other argument to empty values is sufficient for our next step.

##### Run window to wall ratio API

```ruby
# Run a simulation
standard.model_run_simulation_and_log_errors(user_model, run_dir = "#{prototype_dir}/PROP")
# Adjust window to wall ratio
result, wwr_info = standard.model_apply_prm_baseline_window_to_wall_ratio(user_model, climate_zone, wwr_building_type: wwr_building_type)
# Save model
user_model.save(OpenStudio::Path.new("#{prototype_dir}/wwr_output.osm"), true)
```

In the above three lines, the Standard first run an annual simulation of the model to ensure the model is correct and produces a sizing script for analyzing the space conditioning category.
Then the `model_apply_prm_baseline_window_to_wall_ratio` function applies the WWR adjustment to the user energy model.

![multi-building-handling-output](/BEM-for-PRM/user_guide/prm_api_ref/images/multi-building-type-handling-output.PNG)

Open the model in the OpenStudio Application, and you can see the `Perimeter_ZN_2` space is adjusted to `19%` WWR, and the rest are adjusted to `40%` WWR.
