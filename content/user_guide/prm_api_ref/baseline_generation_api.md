---
title: "Baseline generation API"
date: 2022-09-21T15:06:50-07:00
weight: 331
draft: false
pre: "<b>- </b>"
---

- [model_create_prm_stable_baseline_building](#model_create_prm_stable_baseline_building)
  - [model](#--prm-model)
  - [climate_zone](#--climate_zone)
  - [default_hvac_bldg_type](#--default_hvac_bldg_type)
  - [default_wwr_bldg_type](#--default_wwr_bldg_type)
  - [default_swh_bldg_type](#--default_swh_bldg_type)
  - [output_dir](#--output_dir)
  - [unmet_load_hours](#--unmet_load_hours)
  - [debug](#--debug)
- [convert_userdata_csv_to_json](#convert_userdata_csv_to_json)
  - [userdata_dir](#--userdata_dir)
  - [output_dir](#--output_dir)
- [load_userdata_to_standards_database](#load_userdata_to_standards_database)

{{< line_break >}}

### model_create_prm_stable_baseline_building

```ruby
standard = Standard.build("90.1-PRM-2019")

create_results = standard.model_create_prm_stable_baseline_building(model, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, output_dir, unmet_load_hours, debug)
```

The key function that generates the ASHRAE 90.1 PRM model contains many arguments. Many of them have specific enum values. For a successful generation, these arguments need to have exact match to their enum values.
In this instruction, we will walk through the argument list so this API call can be customized for any purposes and building cases.

This API function is a primary function to generate baseline models. The function takes in a user created OpenStudio model and a number of user-defined arguments to generate baseline.

- ##### Step 1: Check proposed model

  {{<mermaid align="center">}}
  graph LR;
  A(Check proposed model) -->|check severe errors and unmet load hours| B{Decision}
  B -->|Passed| C(Create four baseline models)
  B -->|Failed| D(Create one baseline model)
  {{</mermaid>}}
  The simulation results in this step will be saved in the folder `SR_PROP0`.

- ##### Step 2: Adjust envelope and internal loads

  In this step, the function primarily start adjusting the envelope and internal loads based on the information stored in the method arguments and user supplied model. In addition, this step also prepares HVAC systems, adjust the HVAC system for a sizing run.

  {{<mermaid align="left">}}
  graph LR;
  A(Adjust Internal Loads) -->|Remove external shades| B(Shades)
  A -->|Apply compliacne window to wall ratio| C(WWR)
  A -->|Apply compliance skylight to root ratio| D(SRR)
  A -->|Apply compliance lighting power density| E(LPD)
  A -->|Adjust schedule to handle occupancy sensors| F(Schedule)
  A -->|Remove daylighting| G(Daylight)
  A -->|Replace constructions| H(Constructions)
  A -->|Add HVAC and sizing info| I(HVAC)
  {{</mermaid>}}

  After all above is applied, a sizing run is conducted and labeled as `SR1` in the output directory.

- ##### Step 3: Fine tune HVAC system parameters 1

  Based on the sizing run results in the `SR1` folder, this step will fine tuning the HVAC parameters based on the ASHRAE 90.1 appendix G specifications.

  {{<mermaid align="left">}}
  graph LR;
  A(HVAC System) -->|Tune baseline air loop fan power| B(Airloop Fans)
  A -->|Tune baseline zone fan power| C(Zone Fans)
  A -->|Tune boilers| D(Boilers)
  A -->|Tune chillers| E(Chllers)
  A -->|Tune cooling towers| F(CoolingTowers)
  {{</mermaid>}}

  After all above steps are completed, a second sizing run is conducted and labeled as `SR2` in the output directory. In this run, several key metrics in the HVAC system will be determined such as the capacity of the chillers and loop flow rates.

- ##### Step 4: Fine tune HVAC system parameters 2
  In this step, it further fine tune some parameters in the HVAC components
  {{<mermaid align="left">}}
  graph LR;
  A(HVAC Component) -->|Tune component efficiency| B(Efficiency)
  A -->|Set demand control ventilation| C(DCV)
  A -->|Tune pump power and controls| D(Pump)
  {{</mermaid>}}
  A third sizing run is conducted after this step and labeled as `SR3` in the output directory. The third sizing run is the final sizing run to refining size-dependent values including the secondary flow rate in the parallel PIU reheat terminal, and the maximum flow rate in the air terminal.

One or four baseline models will generated in the output directory after these four steps. The output directory is an arugment provided by user when calling this function. In the next section, we will provide a detail explanation of these arguments.

{{< line_break >}}

#### - model

`model` is the OpenStudio model. A typical way to load an OpenStudio model is through the version translator.

```ruby
model_path = '\some_path\some_os_model.osm'
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel(model_path).get
```

{{< line_break >}}

#### - climate_zone

The `climate_zone` shall be one of the strings from the list below:

- `ASHRAE 169-2013-0A`
- `ASHRAE 169-2013-0B`
- `ASHRAE 169-2013-1A`
- `ASHRAE 169-2013-1B`
- `ASHRAE 169-2013-2A`
- `ASHRAE 169-2013-2B`
- `ASHRAE 169-2013-3A`
- `ASHRAE 169-2013-3B`
- `ASHRAE 169-2013-3C`
- `ASHRAE 169-2013-4A`
- `ASHRAE 169-2013-4B`
- `ASHRAE 169-2013-4C`
- `ASHRAE 169-2013-5A`
- `ASHRAE 169-2013-5B`
- `ASHRAE 169-2013-5C`
- `ASHRAE 169-2013-6A`
- `ASHRAE 169-2013-6B`
- `ASHRAE 169-2013-7A`
- `ASHRAE 169-2013-7B`
- `ASHRAE 169-2013-8A`
- `ASHRAE 169-2013-8B`

{{< line_break >}}

#### - default_hvac_bldg_type

The `default_hvac_bldg_type` shall be one of the strings in the list below:

- `heated only storage`
- `hospital`
- `public assembly`
- `retail`
- `other nonresidential`
- `residential`
- `unconditioned`

{{< line_break >}}

#### - default_wwr_bldg_type

The `default_wwr_bldg_type` shall be one of the strings in the list below:

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

{{< line_break >}}

#### - default_swh_bldg_type

The `default_swh_bldg_type` shall be one of the strings in the list below:

- `Workshop`
- `Warehouse`
- `Transportation`
- `Town hall`
- `Sport arena`
- `School/university`
- `Retail`
- `Religious facility`
- `Post office`
- `Police station`
- `Performing arts theater`
- `Penitentiary`
- `Parking garage`
- `Office`
- `Museum`
- `Multifamily`
- `Motion picture theater`
- `Motel`
- `Manufacturing facility`
- `Library`
- `Hotel`
- `Hospital and outpatient surgery center`
- `Health-care clinic`
- `Gymnasium`
- `Grocery store`
- `Fire station`
- `Exercise center`
- `Domitory`
- `Dining:Family`
- `Dining: Cafeteria/fast food`
- `Dining: Bar lounge/leisure`
- `Courthouse`
- `Convention center`
- `Convenience store`
- `Automotive facility`
- `All others`

{{< line_break >}}

#### - output_dir

`output_dir` is a mandatory field that specifies the directory to save all sizing runs. When applying PRM method on a proposed model, OSSTD will perform several sizing runs. These sizing runs usually saved in a directory specified in `sizing_run_dir`. In addition, the generated baseline model `.idf` and `.osm` will also be saved in the this directory.

{{< line_break >}}

#### - unmet_load_hours

`unmet_load_hours` is a flag to check whether the `model` can successfully run a annual simulation and the annual unmet load hours is smaller than 300 hours as required by ASHRAE 90.1 Appendix G. The default value for this flag is `false`, which skips the two checks and run baseline generation.

{{% notice info %}}
Cautious when setting the `unmet_load_hours` to `false`. This could result in wrong baseline models or crash the baseline generation process.
{{% /notice %}}

{{< line_break >}}

#### - debug

`debug` default is set to `false`. This argument has no impact to the current PRM generation and is part of the future implementation. For now, any API calls can set to `false` or simply ignore this argument.

{{< line_break >}}

### convert_userdata_csv_to_json

The function converts user data in csv format to json format and it is typically called before loading the user data before baseline generation.

```
standard = Standard.build("90.1-PRM-2019")
userdata_dir = "#{File.dirname(Dir.pwd)}/user_data"
output_dir = "#{File.dirname(Dir.pwd)}/output"
# Converts user data from .csv file to .json file
json_path = standard.convert_userdata_csv_to_json(userdata_dir, output_dir)
```

The function has two arguments, `userdata_dir` and `output_dir`. The function first load all the default userdata templates as `json`, then grabs all the user csv files saved in the `userdata_dir` and insert data from the `csv` files into the `json` data. Lastly, the function saves the `json` to the `output_dir` in the `user_data_json` folder.

#### - userdata_dir

This argument shall be a valid directory string that pointing to the folder where all user data `csv` are saved. Example can be:

```
"C:\\documents\\my_user_data"
```

#### - output_dir

This argument shall be a valid directory string that pointing to the folder where all user data `json` saved. Example can be:

```
"C:\\documents\\user_data_json"
```

{{< line_break >}}

### load_userdata_to_standards_database

The function loads the user data folder into the `90.1-PRM-2019` standard object. A typical workflow will call `convert_userdata_csv_to_json` first to convert all user data from `csv` to `json` and then call `load_userdata_to_standards_database` to load the json into the workflow.

```
standard = Standard.build("90.1-PRM-2019")
userdata_dir = "#{File.dirname(Dir.pwd)}/user_data"
output_dir = "#{File.dirname(Dir.pwd)}/output"
# Converts user data from .csv file to .json file
json_path = standard.convert_userdata_csv_to_json(userdata_dir, output_dir)
standard.load_userdata_to_standards_database(json_path)
```

`json_path` shall be a valid directory string that pointing to the folder where all user data `json` saved.
