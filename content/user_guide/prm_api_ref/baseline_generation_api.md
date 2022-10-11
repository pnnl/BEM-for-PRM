---
title: "Baseline generation API"
date: 2022-09-21T15:06:50-07:00
weight: 331
draft: false
pre: "<b>- </b>"
---

```ruby
standard = Standard.build("90.1-PRM-2019")

create_results = standard.model_create_prm_stable_baseline_building(model, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, output_dir, unmet_load_hours, debug)
```

The key function that generates the ASHRAE 90.1 PRM model contains many arguments. Many of them have specific enum values. For a successful generation, these arguments need to have exact match to their enum values.
In this instruction, we will walk through the argument list so this API call can be customized for any purposes and building cases.

- [model](#--prm-model)
- [climate_zone](#--climate_zone)
- [default_hvac_bldg_type](#--default_hvac_bldg_type)
- [default_wwr_bldg_type](#--default_wwr_bldg_type)
- [default_swh_bldg_type](#--default_swh_bldg_type)
- [output_dir](#--output_dir)
- [unmet_load_hours](#--unmet_load_hours)
- [debug](#--debug)

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
