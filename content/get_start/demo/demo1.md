---
title: "Demo 1"
date: 2022-08-16T13:04:42-07:00
weight: 242
draft: false
pre: "<b>- </b>"
---

### Demo 1 - Transformation

The proposed model is a typical medium sized office building with roughly `60%` window-to-wall ratio (WWR) and overhangs on its south facade.
![Geometry Transformation Image](/BEM-for-PRM/get_start/os_engine/images/geometry_demo_screenshot.png?width=400px&align=right&classes=border,alignLeft)
{{< line_break >}}

#### Project parameters

First, we will need to check the project parameters in the script.

```Ruby
# Project parameters
default_hvac_bldg_type = 'other nonresidential'
default_wwr_bldg_type = 'Office 5,000 to 50,000 sq ft'
default_swh_bldg_type = 'Office'
climate_zone = 'ASHRAE 169-2013-4A'
```

For this demo, the example is a typical medium sized office building. The `default_hvac_bldg_type` is set to `other nonresidential`, and the window to wall ratio and service water heating system building types are both set according to a related category. The building is assumed to locate in New York City. So its climate zone is `4A`.
These default values shall be assigned with correct strings. All these strings shall match exactly to the designed list.

{{% notice info %}}
You can find the full list of strings for each project parameter [here](../../../user_guide/prm_api_ref/baseline_generation_api/)
{{% /notice %}}

{{< line_break >}}

#### Directories

```Ruby
# directories - change these two variables
demo_folder = 'demo1'
proposed_model = 'geometry_demo_model.osm'

# You may need to change the paths below based on how the project is setup on your local directory:
# This path points to a directory that saves all baseline generation outputs
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
# This path points to the parent folder that contains demo1, demo2 or demo3 folder.
demo_path = "#{File.dirname(Dir.pwd)}/source"
proposed_geo_model = "/#{demo_folder}/#{proposed_model}"
user_data_path = "#{demo_path}/#{demo_folder}/user_data"
```

There are four directories need attentions.

- `baseline_dir` is the path used by PRM method to save all output files.
- `demo_path` is the parent path for demo folder
- `proposed_geo_model` is the relative path of proposed model. The path shall be relative to the `demo_path`
- `user_data_path` is the path to user data folder, which contains all the user data `.csv`.

{{< line_break >}}

#### Initialize

Next we will intialize instances for the baseline generation.

```Ruby
# Script begins
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel("#{demo_path}#{proposed_geo_model}").get
# Generate baseline
standard = Standard.build("90.1-PRM-2019")
```

`model` is the proposed model loaded by `OpenStudio` version translator.
`standard` is the 90.1 PRM 2019 instance of the OpenStudio Standard, which will be used for generating the PRM baseline.

{{< line_break >}}

#### Baseline generation

The last step is to generate the baseline. Before calling the function, there are a few more steps to do.

```Ruby
# Run simulation on proposed model
standard.model_run_simulation_and_log_errors(model, run_dir = "#{baseline_dir}/PROP")
# Load User data
json_path = standard.convert_userdata_csv_to_json(user_data_path, "#{baseline_dir}")
standard.load_userdata_to_standards_database(json_path)
create_results = standard.model_create_prm_stable_baseline_building(model, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, baseline_dir, unmet_load_hours_check=false)
```

- Step 1: Run a simulation on a proposed model. This will generate necessary information for the baseline function
- Step 2: Convert the user data `.csv` to `.json` and load it into the standard instance.
- Step 3: Call the function to generate baseline model.

{{% notice info %}}
You can skip running simulation on proposed model by turning the `unmet_load_hours_check` flag to true.
{{% /notice %}}

{{< line_break >}}

#### Demo 1 results

Noticeably, the generated baseline model has smaller windows (`31%` WWR) and no overhangs.
![Geometry Transformation Image](/BEM-for-PRM/get_start/os_engine/images/geometry_demo1.png?width=400px&align=right&classes=border,alignLeft)

Besides geometry, several building system performances are also updated. Table below shows some transformation examples that can be found in the baseline model.

| Category      | Dependent Variable                         | Standard Requirement       | Baseline Value                                    |
| ------------- | ------------------------------------------ | -------------------------- | ------------------------------------------------- |
| Exterior Wall | Climate Zone 4, nonresidential             | U-0.124 (R-8.06)           | PRM Steel Framed Exetrior Wall R-8.06             |
| Roof          | Climate Zone 4, nonresidential             | U-0.063 (R-15.87)          | PRM IEAD Roof R-15.87                             |
| Window        | Climate Zone 4, nonresidential, 30-40% WWR | U-0.57, SHGC-0.39, Vt-0.43 | PRM U 0.57 SHGC 0.39 VT 0.4 Simple Glazing Window |
| LPD           | Office, whole building                     | 1.10 W/ft2                 | 11.8 W/m2                                         |

**In the next demo, we will demo how to use the user data to handle multiple building types for the same model.**

{{< line_break >}}