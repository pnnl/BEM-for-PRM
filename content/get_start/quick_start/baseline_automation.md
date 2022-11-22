---
title: "Baseline automation"
date: 2022-08-16T13:04:42-07:00
weight: 232
draft: false
pre: "<b>- </b>"
---

This quick tutorial will generate an ASHRAE 90.1-2019 Appendix G baseline from a medium office prototype.

In the previous step, we created a typical medium office model in Climate zone 4.
![saved model](/BEM-for-PRM/get_start/quick_start/image/prototype_medium_office.PNG?width=800px)

In this section, let's write the script to generate a baseline for this prototype building.
First, we need to import `openstudio` and `openstudio-standards` packages.

```ruby
require 'openstudio'
require 'openstudio-standard'
```

Next, load the model and prepare compliance parameters for baseline generation.

```ruby
# Load the openstudio model
model_path = "#{File.dirname(Dir.pwd)}/output_baseline/test_model.osm"
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loaModel(model_path).get

# Baseline parameters
bldg_type = 'MediumOffice'
climate_zone = 'ASHRAE 169-2013-4A'
baseline_dir = "#{File.dirname(Dir.pwd)}/output"
default_hvac_bldg_type = 'other nonresidential'
default_wwr_bldg_type = 'Office 5,000 to 50,000 sq ft'
default_swh_bldg_type = 'Office'
```

Finally, add the script to generate baseline:

```ruby
# load the PRM standard package
standard = Standard.build('90.1-PRM-2019')
P "start generating baseline model ... at #{baseline_dir}"
baseline_models = standard.model_create_prm_stable_baseline_building(model, bldg_type, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, nil, baseline_dir, run_all_orients=true, unmet_load_hours_check=true, debug=false)
```

There are many arguments for the `model_create_prm_stable_baseline_building` function but don't sweat on it. We will cover each one of them in later sections. Nevertheless, this script is sufficient to generate the baseline models for our prototype model.
