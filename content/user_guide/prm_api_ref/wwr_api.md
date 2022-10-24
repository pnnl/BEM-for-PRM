---
title: "Window API"
date: 2022-09-21T15:06:59-07:00
weight: 332
draft: false
pre: "<b>- </b>"
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

{{< line_break >}}

#### Quick Use

```ruby
wwr_building_type = "Office <= 5,000 sq ft"
standard = Standard.build("90.1-PRM-2019")
result, wwr_info = standard.model_apply_prm_baseline_window_to_wall_ratio(model, climate_zone='', wwr_building_type: wwr_building_type)
```

The above code adjust the window to wall ratio in the model to the specified building type. Each building type maps to a window to wall ratio.

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
| `Office 5,00 to 50,000 sq ft`       | 31%                                                                               |
| `Hotel/motel > 75 rooms`            | 34%                                                                               |
| `Hotel/motel <= 75 rooms`           | 24%                                                                               |
| `Hospital`                          | 27%                                                                               |
| `Healthcare (outpatient)`           | 21%                                                                               |
| `All other`                         | 40%                                                                               |

The mapping is shown in the above table. Once the function identifies the correct building area, it will applies the window to wall ratio to the entire model.

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

As the workflow diagram indicated, the function checks each zone and surface to determine its conditioned type. The conditioned type of a zone is determined after model sizing simulation. Therefore, it is critical to note that this function will only work after a successful full year simulation or sizing run. To do that, follow the script below prior calling the function.

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
If the model is not simulated, this function can still run, however, it will not apply a target WWR to the model because it cannot find any conditioned surface to adjust WWR.
{{% /notice %}}

{{< line_break >}}

#### Function behavior

It is important to understand the function behaviors before calling this function.

1. If the target WWR is smaller than the model overall WWR, the function will proportionally reduce the size of every window while keep their center coordinates fixed
2. If the target WWR is greater than the model overall WWR, the function will:
   - remove all windows in surfaces and then add a new windows to meet the required WWR. This step will only applies to surfaces that have windows
   - If every surface that has window is reaching 90% window-to-wall ratio, but the total gross WWR of the building is still smaller than the target WWR, then the function will create new windows in every other surface to meet the target WWR for the building. The size of new windows should be proportional to their host surface.
   - If there are doors in the surface, the maximum window to wall ratio for that surface will readjust to ensure the total area of fenestration and doors to be 90% of the surface area.

{{< line_break >}}

#### Input arguments

##### user_model

`user_model` shall be the OpenStudio model object (`OpenStudio::Model::Model`) that contains a full description of a building energy model in `.osm` format.

##### climate_zone

`climate_zone` is a String data. However it is no longer used in the ASHRAE 90.1 2019 PRM method so it is OK to type in an empty string.

##### wwr_building_type

`wwr_building_type` is a hash key and the value shall be user defined. The default is `nil`. The `wwr_building_type` shall be a string taken from the list below:

- `Warehouse (nonrefrigerated)`
- `School (secondary and university)`
- `School (primary)`
- `Retail (strip mall)`
- `Retail (stand alone)`
- `Restaurant (quick service)`
- `Restaurant (full serivce)`
- `Office > 50,000 sq ft`
- `Office <= 5,000 sq ft`
- `Office 5,00 to 50,000 sq ft`
- `Hotel/motel > 75 rooms`
- `Hotel/motel <= 75 rooms`
- `Hospital`
- `Healthcare (outpatient)`
- `Grocery store`
- `All other`

{{% notice info %}}
It is important keep the `wwr_building_type` string identical to the list above. Otherwise the function will set the target building `WWR` to `40%`.
{{% /notice %}}

{{< line_break >}}

#### Output values

##### result

The result is a boolean. `true` indicates a successful generation and `false` otherwise.

##### wwr_info

The `wwr_info` is a hash map that maps the `wwr_building_type` to its correspondent target `wwr`, which is applied when altering the model window to wall ratio.

```
{"Office <= 5,000 sq ft"=>19.0}
```
