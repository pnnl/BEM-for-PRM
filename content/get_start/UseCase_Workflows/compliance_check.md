---
title: "Compliance Check"
date: 2022-09-21T15:00:28-07:00
weight: 253
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

{{% notice note %}}
This instruction is created based on the latest public information of [ASHRAE Standard 229P](https://tpc.ashrae.org/Documents?cmtKey=9ffa4db6-eebe-4418-a8c4-d0c220603735). It should be noted that the proposed standard is currently under development after its first public review. All information presented in this page can subject to change once the Standard is officially published.
{{% /notice %}}

#### C. Use Case 3: Application of the PRM measure for code compliance check - Ruleset Checking Tool

This use case showcases how an energy modeler can utilize the PRM measure to develop a streamlined automation process for code compliance check. The goal is to automatically generate a proposed and baseline model(s) according to [ASHRAE 90.1 2019 Performance Rating Method (Appendix G)](/BEM-for-PRM/overview/ashrae), and then translate these models and their simulation results into ruleset project description (RPD) files that are compliant with ASHRAE Standard 229. The translated PRD files are used for running compliance rule check through the [ruleset checking tool](https://github.com/pnnl/ruleset-checking-tool) to generate a compliance project check report.

Several tools will be used in this demostration, including PRM measure, [createRulesetProjectDescription](https://github.com/JasonGlazer/createRulesetProjectDescription) and the [PNNL ruleset checking tool](https://github.com/pnnl/ruleset-checking-tool). We also also provide the scripts in this tutorial that automates the process.

{{% notice warning %}}
The [createRulesetProjectDescription](https://github.com/JasonGlazer/createRulesetProjectDescription) and the [PNNL ruleset checking tool](https://github.com/pnnl/ruleset-checking-tool) are under development and they are both highly unstable. The workflow introduced in this page could require updates once both two are released.
{{% /notice %}}

##### **1. Run PRM measure**

There are multiple ways to run the PRM measure. These methods are covered in multiple sections previously. In this instruction, we will use a custom build script to call the PRM through OpenStudio Standard API directly.

```ruby
require 'openstudio'
require 'openstudio-standards'

# Project parameters
default_hvac_bldg_type = 'other nonresidential'
default_wwr_bldg_type = 'Office 5,000 to 50,000 sq ft'
default_swh_bldg_type = 'Office'
climate_zone = 'ASHRAE 169-2013-3A'

# OpenStudio model, and directories
demo_folder = 'demo1'
proposed_model = 'geometry_demo_model.osm'
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
demo_path = "#{File.dirname(Dir.pwd)}/source"
proposed_geo_model = "/#{demo_folder}/#{proposed_model}"
user_data_path = "#{demo_path}/#{demo_folder}/user_data"

# Load the model
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel("#{demo_path}#{proposed_geo_model}").get

# Initialized the Standard instance
# Note: it will need to be 90.1-PRM-2019
standard = Standard.build("90.1-PRM-2019")

# Run simulation on the user model - this is needed for a RPD generation
standard.model_run_simulation_and_log_errors(model, run_dir = "#{baseline_dir}/USER")
# Load User data
json_path = standard.convert_userdata_csv_to_json(user_data_path, "#{baseline_dir}")
standard.load_userdata_to_standards_database(json_path)

# Generate proposed and baseline models
standard.model_create_prm_stable_baseline_building(model, climate_zone, hvac_bldg_type, wwr_bldg_type, swh_bldg_type, baseline_dir, unmet_load_hours_check=true, debug=false)
```

The above ruby script shall be sufficient to complete the PRM generation if you have both OpenStudio and OpenStudio-Standards installed on your computer. You can also check out the [Installation](../../os_engine/installation) page to install the pre-requisite packages.

In this script, we first specifies project parameters that will be used to set up the PRM generation rules. Next, set up the directories to the user model, user data files and output folder. After that, initiaite a `90.1-PRM-2019` Standard instance.
The next step after that is to run a user model simulation. For a successful compliance check, a full year simulation is required on the user model.
If there are user provided data, such as lighting space type, you will need to load those data using the `load_userdata_to_standards_database`. If those user data are in `.csv` format, you will also need to call `convert_userdata_csv_to_json` first.
Lastly, call the PRM measure method, `model_create_prm_stable_baseline_building` function to generate proposed and baseline models. To have a successful compliance check, we will need to turn on the `unmet_load_hours_check`. This flag will not only run full year simulations on proposed and baseline models, but also export simulation results in jsons - a critical format for generating ruleset project description file.

{{<line_break>}}

##### **2. Generate RPD files and run evaluation**

Before running this task, check the output folder. You should be seeing files ends with `.epjson`. Inside each simulation folder, you should also see `eplusout_hourly.json`.
After verification, the next step is to ensure the package [createRulesetProjectDescription](https://github.com/JasonGlazer/createRulesetProjectDescription). The simplest installation method is to have their source code downloaded on your local directory. Make sure that you have read the requirements of the installation from each package, for example, the version of python, and follows their guideline to run the source code on test cases.

Once the two software installed or downloaded on local directory, we can then develop another script to glue all the processes together.

```python
# We need to import the createRulesetModelDescription in the script as the tool does not
# provide a CLI function yet.
sys.path.append("[YOUR_LOCAL_DIRECTORY]\\createRulesetModelDescription")
import subprocess
import energyplus_rpd.translator import Translator
import pathlib import Path
RCT_DIR = "[YOUR_LOCAL_DIRECTORY]\\ruleset-checking-tool\\"

user_model_path = "[directory_to_your_user_model]"
proposed_model_path = "[directory_to_your_proposed_model]"
baseline_model_path = "[directory_to_your_baseline_model]"

user_translator = Translator(Path(f"{user_model_path}/user_model.epJSON"), f"{user_model_path}/user_rpd.rpd")
user_translator.process()
proposed_translator = Translator(Path(f"{proposed_model_path}/proposed_model.epJSON"), f"{proposed_model_path}/proposed_rpd.rpd")
proposed_translator.process()
baseline_translator = Translator(Path(f"{baseline_model_path}/baseline_model.epJSON"), f"{baseline_model_path}/baseline_rpd.rpd")
baseline_translator.baseline()

# call RCT evaluation throught its CLI

subprocess.run(["pipenv",
                "run",
                "rct229", # application name
                "evaluate", # function name
                "-f", f"{user_model_path}/user_rpd.rpd", # -f command to append an RPD file to the evalute function
                "-f", f"{baseline_model_path}/baseline_rpd.rpd",
                "-f", f"{proposed_model_path}/proposed_rpd.rpd",
                "-rs", "ashrae9012019", # -rs command specifies the ruleset
                "-r", "ASHRAE9012019_DETAIL",  # -r command specifies the type of report(s)
                "-rd", "./example/output/"], # -rd command specifies the output report saving directory
                cwd=RCT_DIR,
                shell=True
                )
```

The above script does two steps:

1. Utilize the Translator class in the `createRulesetModelDescription` application to translate the EnergyPlus simulation files, including `.epJSON`, `_modelout.json` and `_modelout_hourly.json`, to the RPD files. For this specific instance, we translated the `user`, `proposed` and `baseline` models. For many 90.1 2019 code compliance projects, baseline models in four orientations may need translation individually.
2. Run RPD evaluations through the RCT tool's command line interface. For a project evaluation, the function is called `evaluate`. Arguments to this function includes RPD file paths (`-f`), ruleset option (`-rs`) and evaluation report (`-r`). The application will run an evaluation based on the specifications and produce one or multiple reports.

In the end, we should found an output file `ashrae901_2019_detail_report.json` in the `./example/output` folder. The schema of this detail report is published on the [ruleset checking tool](https://github.com/pnnl/ruleset-checking-tool/blob/master/docs/_output_project_test_report_template.md) website. In this file, you will be able to identify outcomes that are not showing `PASS` to address the compliance issues.
