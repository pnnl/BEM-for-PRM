---
title: Run the measure
date: 2022-09-21T14:50:40-07:00
weight: 222
draft: false
pre: "<b>- </b>"
---

## Prerequisite

Before running any measures, make sure you have OpenStudio SDK installed. The instruction and correct version can be found in [Get OpenStudio SDK](../../os_cli/get_openstudio_sdk).

{{< line_break >}}

## Download the PRM measure

Download measure in the link below:
{{%attachments title="Create Baseline Building:" style="orange" pattern=".*\.(zip)$"/%}}

Refer to [Install the measure](../../os_app/install_measure) for other installation options.

{{< line_break >}}

## Setup

Setup a local directory to include all required inputs (the following example is one way to setup the directory):

> - **Seed model**: place the `.osm` file in a directory named `files`. The Openstudio model should meet the [model requirements](../../../user_guide/model_requirements/).
> - **Weather file**: place the `.epw` file in a directory named `weather`.
> - **Measure**: place the `CreateBaselineBuilding` measure in a directory called `measures`.
> - **OS Workflow**: place the `.osw` file in the main directory.
> - **User Data (_Optional_)**: place the `.csv` file in the main directory (same level as the `.osw`). The use of the user data can be found in the [add compliance data section](../../../user_guide/add_compliance_data/).

The image below shows one way to setup the main directory and their corresponding sub-directories.

![Directory Setup](/BEM-for-PRM/get_start/os_cli/images/folder_structure.png?width=800px&align=left&classes=border)

{{< line_break >}}

## Edit OSW file

The OSW file can be structured as shown in the image below.

> - Lines 2-3 define the seed model and weather file.
> - Lines 4-5 define the measure path.
> - Lines 9-14 contain arguments to be passed to the measure. A detailed description of the arguments can be found in the [API document](../../../user_guide/prm_api_ref/baseline_generation_api/).

![OS Workflow Structure](/BEM-for-PRM/get_start/os_app/images/osw.JPG?width=600px&align=left&classes=border)

{{% notice tip %}}
**Important Note:**
The directory names are not case sensitive. If you want to change the names of the directories that contain the seed model and weather file, make sure to define the paths in the OSW file.
{{% /notice %}}

{{< line_break >}}

## Execution

- Open Command Prompt as an administrator.
- Type the following command with paths to the **openstudio\bin**, **openstudio-standard\lib** and the **OSW** directories.  
  `C:\openstudio-3.6.0\bin\openstudio.exe run -w "[LOCAL_PATH]\workflow.osw"`

**Example:**

```ruby
C:\openstudio-3.6.0\bin\openstudio.exe  run -w "C:\Users\sample_user\baselinePRM\test.osw"
```

{{% notice tip %}}
If the openstudio command is not recognized, either add the executable to your environment PATH or add the full path to the `openstudio-3.6.0\bin` directory.
It is advised to avoid spaces in file and directory names. Use quotation marks when specifying long file names or paths with spaces.
{{% /notice %}}

{{% notice tip %}}
The OpenStudio SDK version needs to be **3.5.0** or later.
{{% /notice %}}

{{< line_break >}}

## Output

Once the workflow simulation is **completed**, you will find additional files generated in your project directory.

- An output OSW file `out.osw` is written to indicate whether the simulation run was successful or not.

Once the workflow is completed **unsuccessfully (fails)**:

- A `run` directory is generated with a `run.log` file that contains the error message.

Once the workflow is completed **successfully**:

- A `run` directory is generated which contains all input/output files.
- A `reports` directory is created which contains energyplus's standard html report.
- A `generated_files` directory contains all the generated models and simulation information.
- A `user_data_json` directory contains all user provided data in `json` format.

![Measure Output Structure](/BEM-for-PRM/get_start/os_cli/images/output_files.PNG?width=800px&align=left&classes=border)
