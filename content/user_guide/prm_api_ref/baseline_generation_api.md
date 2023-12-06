---
title: "Baseline generation API"
date: 2022-09-21T15:06:50-07:00
weight: 331
draft: false
pre: "<b> - </b>"
---

{{<line_break>}}

#### The API functions for baseline generation including its arguments are as listed

- [model_create_prm_stable_baseline_building](#function-1-model_create_prm_stable_baseline_building)
  - [model](#argument-1-model)
  - [climate_zone](#argument-2-climate_zone)
  - [default_hvac_bldg_type](#argument-3-default_hvac_bldg_type)
  - [default_wwr_bldg_type](#argument-4-default_wwr_bldg_type)
  - [default_swh_bldg_type](#argument-5-default_swh_bldg_type)
  - [output_dir](#argument-6-output_dir)
  - [unmet_load_hours](#argument-7-unmet_load_hours)
  - [debug](#argument-8-debug)


Read the section below for a detailed breakdown of each function. 

{{< line_break >}}

#### FUNCTION 1: model_create_prm_stable_baseline_building
This is the primary API function to generate baseline models. The process involves taking a user-created OpenStudio model along with several user-defined arguments to produce a PRM baseline model. This function contains numerous arguments, many of which require specific enum values. It's crucial that these arguments align with their corresponding enum values.

```ruby
standard = Standard.build("90.1-PRM-2019")

create_results = standard.model_create_prm_stable_baseline_building(model, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, output_dir, unmet_load_hours, debug)
```

Here is a breakdown of steps involved in the "model_create_prm_stable_baseline_building" function.

- **Step 1: Generate a proposed model**
  {{<mermaid align="center">}}
  graph LR;
  A(User Model) -->|check severe errors| B{Decision}
  B -->|Passed| C(Creates a proposed model)
  B -->|Failed| D(Generates an error)
  {{</mermaid>}}


- **Step 2: Check proposed model**
  {{<mermaid align="center">}}
  graph LR;
  A(Check proposed model) -->|check severe errors and unmet load hours| B{Decision}
  B -->|Passed| C(Create four baseline models)
  B -->|Failed| D(Create one baseline model)
  {{</mermaid>}}
  The simulation results in this step will be saved in the folder `SR_PROP0`.
 
- **Step 3: Adjust envelope and internal loads**
  
  In this step, the function starts adjusting the envelope and internal loads based on the information stored in the method arguments and user-supplied model. In addition, this step also prepares HVAC systems for a sizing run.

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

  After all the above is applied, a sizing run is conducted and labeled as `SR1` in the output directory.

- **Step 4: Fine tune HVAC system parameters 1**

  Based on the sizing run results in the `SR1` folder, this step will fine-tune the HVAC parameters based on the ASHRAE 90.1 Appendix G specifications.

  {{<mermaid align="left">}}
  graph LR;
  A(HVAC System) -->|Tune baseline air loop fan power| B(Airloop Fans)
  A -->|Tune baseline zone fan power| C(Zone Fans)
  A -->|Tune boilers| D(Boilers)
  A -->|Tune chillers| E(Chllers)
  A -->|Tune cooling towers| F(CoolingTowers)
  {{</mermaid>}}

  After all the above steps are completed, a second sizing run is conducted and labeled as `SR2` in the output directory. In this run, several key metrics in the HVAC system will be determined, such as the capacity of the chillers and loop flow rates.

- **Step 5: Fine tune HVAC system parameters 2**

  This step further fine-tunes some parameters in the HVAC components.
  {{<mermaid align="left">}}
  graph LR;
  A(HVAC Component) -->|Tune component efficiency| B(Efficiency)
  A -->|Set demand control ventilation| C(DCV)
  A -->|Tune pump power and controls| D(Pump)
  {{</mermaid>}}
  A third sizing run is conducted after this step and labeled as `SR3` in the output directory. The third sizing run is the final sizing run to refine size-dependent values, including the secondary flow rate in the parallel PIU reheat terminal and the maximum flow rate in the air terminal.

After these four steps, baseline model(s) will be generated in the output directory. The output directory is an argument the user provides when calling this function. 
Below you can find a detailed explanation of these arguments, so this API call can be customized for any purpose and building cases.

##### **ARGUMENT 1: model**

`model` is the OpenStudio model. A typical way to load an OpenStudio model is through the version translator.

```ruby
model_path = '\some_path\some_os_model.osm'
translator = OpenStudio::OSVersion::VersionTranslator.new
model = translator.loadModel(model_path).get
```

##### **ARGUMENT 2: climate_zone**

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

##### **ARGUMENT 3: default_hvac_bldg_type**

The `default_hvac_bldg_type` shall be one of the strings in the list below:

- `heated only storage`
- `hospital`
- `public assembly`
- `retail`
- `other nonresidential`
- `residential`
- `unconditioned`

##### **ARGUMENT 4: default_wwr_bldg_type**

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
- `Office 5,000 to 50,000 sq ft`
- `Hotel/motel > 75 rooms`
- `Hotel/motel <= 75 rooms`
- `Hospital`
- `Healthcare (outpatient)`
- `Grocery store`
- `All other`

##### **ARGUMENT 5: default_swh_bldg_type**

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

##### **ARGUMENT 6: output_dir**

`output_dir` is a mandatory field that specifies the directory to save all sizing runs. When applying the PRM method to a proposed model, OSSTD will perform several sizing runs. These sizing runs are usually saved in a directory specified in `sizing_run_dir`. In addition, the generated baseline model `.idf` and `.osm` will also be saved in this directory.

##### **ARGUMENT 7: unmet_load_hours**

`unmet_load_hours` is a flag that checks whether a `model` can successfully run an annual simulation and whether its annual unmet load hours meet the ASHRAE 90.1 Appendix G requirements. The default value for this flag is `true`.

{{% notice info %}}
Cautious when setting the `unmet_load_hours` to `false`. This could result in wrong baseline models or crash the baseline generation process.
{{% /notice %}}

##### **ARGUMENT 8: debug**

`debug` default is set to `false`. This argument has no impact on the current PRM generation and is part of the future implementation. Any API calls set this field to `false` or ignore this argument.


