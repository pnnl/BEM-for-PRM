---
title: "Demo Files"
date: 2022-08-16T13:04:42-07:00
weight: 241
draft: true
pre: "<b>- </b>"
---

Each zip file contains the model, user data and weather file for performing demos.

#### **Demo Files**

- [demo1_zip](/BEM-for-PRM/get_start/demo/quick_start.files/demo1.zip): Transformations

- [demo2_zip](/BEM-for-PRM/get_start/demo/quick_start.files/demo2.zip): User Data

- [demo3_zip](/BEM-for-PRM/get_start/demo/quick_start.files/demo3.zip): HVAC

{{< line_break >}}
#### **Preparation**

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

We hope the examples provide some ideas and inspirations on how to use the PRM method. For detail API explainations, check out the [API document](../../../user_guide/prm_api_ref/baseline_generation_api/)