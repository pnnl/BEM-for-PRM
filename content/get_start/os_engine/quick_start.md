---
title: "Examples"
date: 2022-09-21T15:01:04-07:00
weight: 234
draft: false
pre: "<b>- </b>"
---

In this chapter, we will cover three examples to showcase the capability of the OSSTD-PRM method.

- [Demo files](#demo-files)
- [Preparation](#preparation)
- [Demo 1: Transformations](#demo-1---transformation)
  - [Project parameters](#project-parameters)
  - [Directories](#directories)
  - [Initialize](#initialize)
  - [Baseline generation](#baseline-generation)
  - [Demo 1 results](#demo-1-results)
- [Demo 2: User data](#demo-2-user-data)
  - [User data](#user-data)
  - [Demo 2 results](#demo-2-results)
- [Demo 3: HVAC](#demo-3-hvac)
  - [Demo 3 results](#demo-3-results)

{{< line_break >}}

### Demo files

Each zip file contains the model, user data and weather file for performing demos.

{{%attachments title="Demo zip files" style="orange" pattern=".*\.(zip)$"/%}}

{{< line_break >}}

### Preparation

You will need the script below for running the demo case. We will later explain each code block in the script.

```Ruby
require 'openstudio'
require 'openstudio-standards'

# Project parameters
default_hvac_bldg_type = 'other nonresidential'
default_wwr_bldg_type = 'Office 5,000 to 50,000 sq ft'
default_swh_bldg_type = 'Office'
climate_zone = 'ASHRAE 169-2013-4A'

# directories - change these two variables
demo_folder = 'demo1'
proposed_model = 'geometry_demo_model.osm'

baseline_dir = "#{File.dirname(Dir.pwd)}/output"
demo_path = "#{File.dirname(Dir.pwd)}/source"
proposed_geo_model = "/#{demo_folder}/#{proposed_model}"
user_data_path = "#{demo_path}/#{demo_folder}/user_data"

# Script begins
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel("#{demo_path}#{proposed_geo_model}").get
# Generate baseline
standard = Standard.build("90.1-PRM-2019")

# Run simulation on proposed model
standard.model_run_simulation_and_log_errors(model, run_dir = "#{baseline_dir}/PROP")
# Load User data
json_path = standard.convert_userdata_csv_to_json(user_data_path, "#{baseline_dir}")
standard.load_userdata_to_standards_database(json_path)
create_results = standard.model_create_prm_stable_baseline_building(model, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, baseline_dir, unmet_load_hours_check=false)
```

Making sure you have `openstudio` and `openstudio-standards` installed on your local environment. The installation steps can be found in [installation](../installation).

{{% notice warning %}}
It is important to let OSM file pointing to the correct weather file directory. You can find the path in the `.OSM` file, `OS:WeatherFile,` data group. The default weather file path is `../demo[X]/USA_NY_New.York-J.F.Kennedy.Intl.AP.744860_TMY3.epw`, which is pointing to the weather file under the folder `demo[X]`.
{{% /notice %}}

{{< line_break >}}

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

In the next demo, we will demo how to use the user data to handle multiple building types for the same model.

{{< line_break >}}

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

In the next demo, we will show an HVAC transformation example.

{{< line_break >}}

### Demo 3: HVAC

In the last demo, we have a medium office equipped with water source heat pumps for heating and cooling and a DOAS for ventilation.
Also, the **Core Top Zone** is being identified as a high equipment density (`40 W/m2`) room.

According to the ASHRAE 90.1 PRM method, the baseline system type for the medium office shall be a packaged VAV with reheat (system 5) and the **Core Top Zone** shall be separatedly conditioned by a packaged single zone air conditioner (system 3) according to the exception c.
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

Hope the above examples provide some ideas and inspirations on how to use the PRM method. For detail API explainations, check out the [API document](../../../user_guide/prm_api_ref/baseline_generation_api/)
