---
title: "Create a typical model"
date: 2022-07-26T15:43:57-07:00
weight: 232
draft: false
pre: "<b>- </b>"
---

### Create a typical model

OpenStudio Standard package embedded code to generate OpenStudio version of prototype buildings. This quick tutorial will create a typical building and run the ASHRAE 90.1-2019 Appendix G (PRM) automation process.

To begin with, we need to import both packages.

```ruby
require 'openstudio'
require 'openstudio-standard'
```

{{% notice info %}}
You can use `require_relative [ABSOLUTE_PATH]` to import the OSSTD source code if you choose to use the package from Github.
{{% /notice %}}

Next, we want to create a `MediumOffice` prototype building in climate zone 4A (New York) that is designed following the 90.1 2019 standard. Before doing that, we will need to define these parameters.

```ruby
code_version = '90.1-2019'
prototype_template = 'MediumOffice'
climate_zone = 'ASHRAE 169-2013-4A'
epw_file = 'USA_NY_New.York-J.F.Kennedy.Intl.AP.744860_TMY3.epw'
prototype_dir = "#{File.dirname(Dir.pwd)}/output" # path where the generated prototype saved.
```

The next step is to generate the typical medium office model

```ruby
standard = Standard.build("#{code_version}_#{prototype_template}")
model = standard.model_create_prototype_model(climate_zone, epw_file, prototype_dir)
```

The first line loads a specific OpenStudio Standard object based on the code version (`90.1-2019`) and the prototype template (`MediumOffice`).
The second line calls `model_create_prototype_model` method from the object with arguments include `climate_zone`, `epw_file` and `prototype_dir` to create the prototype model. This method will create a path in the computer to store the draft prototype model and sizing run of the model. Then it will fine-calibrate the model based on the sizing outcome to produce the final version, which is the returned value from this method.

Last, we can save the generated prototype model to a directory and view it in the OpenStudio Application.

![saved model](/BEM-for-PRM/get_start/quick_start/image/prototype_medium_office.PNG?width=800px)

The full script of this part of the tutorial is provided below:

```ruby
require 'openstudio'
# This imports the source code of the openstudio standard. If OSSTD is installed in GEM, then simply
# use require 'openstudio-standards'
require_relative File.join('..','..','..', 'OSSTD_Repo','lib','openstudio-standards.rb')

code_version = '90.1-2019'
prototype_template = 'MediumOffice'
climate_zone = 'ASHRAE 169-2013-4A'
epw_file = 'C:\WeatherData\USA_NY_New.York-J.F.Kennedy.Intl.AP.744860_TMY3.epw'
prototype_dir = "#{File.dirname(Dir.pwd)}/output"

standard = Standard.build("#{code_version}_#{prototype_template}")
model = standard.model_create_prototype_model(climate_zone, epw_file, prototype_dir)

# Save prototype OSM file
osm_path = OpenStudio::Path.new("#{prototype_dir}/test_model.osm")
model.save(osm_path, true)
```

Next, we will write a script to automatically generate ASHRAE 90.1 Appendix G (PRM) model from the generated prototype model. Click the Next button at the right edge to start the script.
