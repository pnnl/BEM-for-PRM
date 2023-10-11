---
title: "Demo 3: User Data"
date: 2022-08-16T13:04:42-07:00
weight: 244
draft: false
pre: "<b>- </b>"
---
{{<line_break>}}

#### Demo 3 shows how to provide additional user data for the PRM function

In Demo 3, we will demonstrate how to input user data to handle multiple building types in a model.

Download the folder "demo3_zip" on your system and extract it in any local folder (*_to be changed based on the method used in the sections below_*). The zip file contains the model (.osm), user data (.csv) and weather file (.epw) to follow along the demo.
- **[demo3_zip](/BEM-for-PRM/get_start/demo/quick_start.files/demo3.zip): User Data**

It is common for a building to have multiple types, for example, the first floor of the medium office can function as retail spaces and the rest of the floors are offices.
In this case, we will need to provide more information for the PRM function so it can properly handle the multi-building types scenario.
To do that, we will need to use the *_userdata_space.csv_*. In the `demo3` zip file, there is a `user_data` folder. Inside this folder, we have a `userdata_space.csv` file. Let's open the file to inspect.

In the file (as shown in the image), we can see all perimeter zones on the first floor are set to `Retail (strip mall)` WWR building type. For lighting space type, the `Perimeter_bot_ZN_2` is set to `retail mall concourse` and the other spaces are set to `retail - whole building`. With this file, we will expect the first floor perimeter spaces have different window to wall ratios and lighting power densities than the other spaces in the building.

![userdata](/BEM-for-PRM/get_start/os_engine/images/demo2_space_userdata.PNG?width=700px&align=right&classes=border,alignLeft)

In the following sections, follow the steps to run the demo file based on the method you will be using. 
- [Using OS App](/BEM-for-PRM/get_start/demo/demo3/#a-using-os-app)
- [Using OS SDK with CLI](/BEM-for-PRM/get_start/demo/demo3/#b-using-os-sdk-using-cli)
- [Using OS SDK API](/BEM-for-PRM/get_start/demo/demo3/#c-using-os-sdk-api)

{{< line_break >}}

##### **A. Using OS App**

1. **Launch OpenStudio Application (OS App):** It should be installed in your system, if not, make sure you have followed all the steps in [Use with OS App](/BEM-for-PRM/get_start/os_app/) section. 

2. **Load the .osm file:** Go to the File menu and select Open. Navigate to the folder `demo 2` and select `geometry_demo_model.osm` file. The project inputs like weather file, climate zone and location will load automatically (as shown in the image). If any parameter is missing, add it manually. 
![demo1_loadosmfile image](/BEM-for-PRM/get_start/demo/images/demo1_loadosmfile.PNG?width=1400px&align=right&classes=border,alignLeft)

3. **Add the measure:** Go to the Measures tab in the left menu. Under Whole Building > Space Types, *_Create ASHRAE 90.1 2019 PRM Model_* measure should be available. If you don't see it, revisit the steps in [Use with OS App](/BEM-for-PRM/get_start/os_app/) section. Add the measure.

4. **Setting Project Parameters:** Click on the measure after adding it, a few project parameters will show up in the **Edit** menu on the right. Under **Inputs**, select the following: 
    - Default Building Area Type for Window To Wall Ratio : **_Office 5,000 to 50,000 sq ft_**
    - Default Building Area Type for Service Water Heating : **_Office_**
    - Default Building Type for HVAC : **_other nonresidential_**
    - Climate Zone : **_ASHRAE 169-2013-4A_**
    - Exempt from Rotations : **_FALSE_** (You can select TRUE for your project if the building has rating authority approval that orientation is dictated by site considerations.)
    - Exempt from Unmet Load Hours Check : **_TRUE_** (TRUE has been selected here only for demo purposes, otherwise it's always recommended to select FALSE until and unless the rating authority has approved allowing the models to exceed the specified.)
    - Use User Data : **_TRUE_** (TRUE selected since we are going to use the user data for this demo.) 
    - Input the absolute path for userdata_space.csv in the field provided. <!--to be updated in November-->

5. **Run Simulation:** Go to the **Run Simulation** tab and run the  simulation. Once it's completed, go to the *_demo2_* folder in the system. A new folder *_geometry_demo_model_* would be created containing subfolders for the runs as shown in [Run the measure](/BEM-for-PRM/get_start/os_app/how_run_measure/) section. The `generated_files` folder would contain the output .osm files, reports that should reflect the Output shown [here](/BEM-for-PRM/get_start/demo/demo3/#demo-3-results).  

{{<line_break>}}

##### **B. Using OS SDK using CLI**

1. **Setup Local Directory:** Set up the files from `demo3` folder in a local directory. One such example of the folder structure is shown in [Run the measure](/BEM-for-PRM/get_start/os_cli/run_the_measure/) section. For this demo, place the userdata_space.csv in the directory as well.
2. **Edit OSW File:** Open the .osw file created initially (Refer to [Run the measure](/BEM-for-PRM/get_start/os_cli/run_the_measure/) section). Update the project inputs and parameters as follows. 
![CLI Method osw file structure](/BEM-for-PRM/get_start/demo/images/demo1_CLImethod_oswfile.PNG?width=600px&align=right&classes=border,alignLeft) 
3. **Execute:** Open Command Prompt as an administrator and type the following command with paths to the openstudio\bin and the OSW directories.

    ```ruby
    C:\openstudio-3.6.1\bin\openstudio.exe  run -w "C:\Users\sample_user\demo3\test.osw"
    ```
   {{% notice info %}}
   - Make sure you change the *_openstudio version_* based on what is installed on your system, for example 3.6.0 instead of 3.6.1.
   - *_sample_user* to be replaced by your user name in C: drive. 
   - *_demo3_* to be replaced by the folder path where test.osw is located.
   {{% /notice %}}

4. **Output:** Once the simulation is run, go to the demo3 folder (or the folder where you had all the files set up in the local directory) to access the output files as shown in [Run the measure](/BEM-for-PRM/get_start/os_cli/run_the_measure/) section. The `generated_files` folder would contain the output .osm files, reports that should reflect the Output shown [here](/BEM-for-PRM/get_start/demo/demo3/#demo-3-results). 
 

{{<line_break>}}

##### **C. Using OS SDK API**

1. Open the Ruby file created in the [Call measure via API](/BEM-for-PRM/get_start/os_engine/call_use_api/) section. 
2. **Check Project parameters:** Check and update the project parameters in the script as follows:

   ```Ruby
    # Project parameters
    default_hvac_bldg_type = 'other nonresidential'
    default_wwr_bldg_type = 'Office 5,000 to 50,000 sq ft'
    default_swh_bldg_type = 'Office'
    climate_zone = 'ASHRAE 169-2013-4A'
    ```
    These default values shall be assigned with correct strings. All these strings shall match exactly to the designed list.

{{% notice info %}}
You can find the full list of strings for each project parameter [here](../../../user_guide/prm_api_ref/baseline_generation_api/)
{{% /notice %}}


3. **Set up Directories:** There are four directories that must be setup. 
      - `baseline_dir` is the path used by PRM method to save all output files.
      - `demo_path` is the parent path for demo folder
      - `proposed_geo_model` is the relative path of proposed model. The path shall be relative to the `demo_path`
      - `user_data_path` is the path to user data folder, which contains all the user data `.csv`.

   ```Ruby
    # directories - change these two variables
    demo_folder = 'demo3'
    proposed_model = 'geometry_demo_model.osm'

    # You may need to change the paths below based on how the project is setup on your local directory:
    # This path points to a directory that saves all baseline generation outputs
    baseline_dir = "#{File.dirname(Dir.pwd)}/output"
    # This path points to the parent folder that contains demo1, demo2 or demo3 folder.
    demo_path = "#{File.dirname(Dir.pwd)}/source"
    proposed_geo_model = "/#{demo_folder}/#{proposed_model}"
    user_data_path = "#{demo_path}/#{demo_folder}/user_data"
    ```

4. **Initialize:** Next we will intialize instances for the baseline generation.

    ```Ruby
    # Script begins
    translator = OpenStudio::OSVersion::VersionTranslator.new
    model = translator.loadModel("#{demo_path}#{proposed_geo_model}").get
    # Generate baseline
    standard = Standard.build("90.1-PRM-2019")
    ```

   `model` is the proposed model loaded by `OpenStudio` version translator.
`standard` is the 90.1 PRM 2019 instance of the OpenStudio Standard, which will be used for generating the PRM baseline.

5. **Baseline generation:** The last step is to generate the baseline. Before calling the function, there are a few more steps to do.

    - Step 1: Run a simulation on a proposed model. This will generate necessary information for the baseline function
    - Step 2: Convert the user data `.csv` to `.json` and load it into the standard instance.
    - Step 3: Call the function to generate baseline model. 

    ```Ruby
    # Run simulation on proposed model
    standard.model_run_simulation_and_log_errors(model, run_dir = "#{baseline_dir}/PROP")
    # Load User data
    json_path = standard.convert_userdata_csv_to_json(user_data_path, "#{baseline_dir}")
    standard.load_userdata_to_standards_database(json_path)
    create_results = standard.model_create_prm_stable_baseline_building(model, climate_zone, default_hvac_bldg_type, default_wwr_bldg_type, default_swh_bldg_type, baseline_dir, unmet_load_hours_check=false)
    ```



{{% notice info %}}
You can skip running simulation on proposed model by turning the `unmet_load_hours_check` flag to true.
{{% /notice %}}

{{<line_break>}}

#### Demo 3 results

The WWR on the first floor should be decreased than the WWR on the second and third floor. This is because the ASHRAE 90.1 PRM requires the `Retail (strip mall)` WWR to be `19%`, which is much smaller than the `Office 5,000 to 50,000 sq ft` (`31%`).

   ![Geometry demo 2](/BEM-for-PRM/get_start/os_engine/images/demo2_geometry_screenshot.png?width=400px&align=right&classes=border,alignLeft)

   Besides WWR, several building system performances are also updated. Table below shows some transformation examples that can be found in the baseline model.

   | Category      | Apply to                       | Dependent Variable                         | Standard Requirement       | Baseline Value                                    |
   | ------------- | ------------------------------ | ------------------------------------------ | -------------------------- | ------------------------------------------------- |
   | Exterior Wall | All                            | Climate Zone 4, nonresidential             | U-0.124 (R-8.06)           | PRM Steel Framed Exetrior Wall R-8.06             |
   | Roof          | All                            | Climate Zone 4, nonresidential             | U-0.063 (R-15.87)          | PRM IEAD Roof R-15.87                             |
   | Window        | All                            | Climate Zone 4, nonresidential, 30-40% WWR | U-0.57, SHGC-0.39, Vt-0.43 | PRM U 0.57 SHGC 0.39 VT 0.4 Simple Glazing Window |
   | LPD           | Spaces excluded from user data | Office, whole building                     | 1.10 W/ft2                 | 11.8 W/m2                                         |
   | LPD           | First floor 1, 3, and 4        | User data: retail - whole building         | 1.50 W/ft2                 | 16.1 W/m2                                         |
   | LPD           | First floor 2                  | User data: retail mall concourse           | 1.70 W/ft2                 | 18.3 W/m2                                         |

- *_Dependent Variable: The variables that determine the changes from a user model to a baseline model_*
- *_Standard Requirement: The requirements mentioned in the ASHRAE 90.1 2019 PRM method for a baseline model_*
- *_Baseline Value: The values assigned to the generated baseline model to match the Standard Requirements (Please note the units for LPD are different)_* 

{{< line_break >}}

