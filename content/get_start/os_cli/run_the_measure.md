---
title: Run the measure
date: 2022-09-21T14:50:40-07:00
weight: 222
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

<!-- ## Prerequisite

Before running any measures, make sure you have OpenStudio SDK installed. The instruction and correct version can be found in [Get OpenStudio SDK](../../os_cli/get_openstudio_sdk).

{{< line_break >}}
-->

#### B. Run the measure with OS SDK using CLI

There are 5 parts in the process of running the measure.

1.  [**Download the PRM measure (prerequisite)**](#1-download-the-prm-measure)
2.  [**Setup a local directory**](#2-setup-a-local-directory)
3.  [**Edit OSW file**](#3-edit-osw-file)
4.  [**Execute the measure**](#4-execute-the-measure)
5.  [**Save the Output**](#5-save-the-output)

{{< line_break >}}

##### **1. Download the PRM measure**

Download measure in the link below:
{{%attachments title="Create Baseline Building:" style="orange" pattern=".*\.(zip)$"/%}}

Refer to [Install the measure](../../os_app/install_measure) for other installation options.

{{< line_break >}}

##### **2. Setup a local directory**

Setup a local directory to include all required inputs (the following example is one way to setup the directory):

> - **Seed model**: place the `.osm` file in a directory named `files`. The Openstudio model should meet the [model requirements](../../../user_guide/model_requirements/).
> - **Weather file**: place the `.epw` file in a directory named `weather`.
> - **Measure**: place the `Create ASHRAE 90.1 2019 PRM Model` measure in a directory called `measures`.
> - **OS Workflow**: place the `.osw` file in the main directory.
> - **User Data (_Optional_)**: place the `.csv` file in the main directory (same level as the `.osw`). The use of the user data can be found in the [add compliance data section](../../../user_guide/add_compliance_data/).

The image below shows one way to setup the main directory and their corresponding sub-directories. This can be set up in any other way, however, this structure will be followed for all examples provided in this documentation. Thus, it's recommended to make necessary changes if the folders are set up in a different manner.

![Directory Setup](/BEM-for-PRM/get_start/os_cli/images/folder_structure.png?width=800px&align=left&classes=border)

{{< line_break >}}

##### **3. Edit OSW file**

The OSW file can be structured as shown in the image below.

> - Lines 2-3 define the seed model and weather file.
> - Lines 4-5 define the measure path.
> - Lines 9-14 contain arguments to be passed to the measure. A detailed description of the arguments can be found in the [API document](../../../user_guide/prm_api_ref/baseline_generation_api/).

![OS Workflow Structure](/BEM-for-PRM/get_start/os_app/images/osw.JPG?width=800px&align=left&classes=border)

{{% notice tip %}}
**Important Note:**
The directory names are not case sensitive. If you want to change the names of the directories that contain the seed model and weather file, make sure to define the paths in the OSW file.
{{% /notice %}}

{{< line_break >}}

##### **4. Execute the measure**

- Open Command Prompt as an administrator.
- Type the following command with paths to the **openstudio\bin**, **openstudio-standard\lib** and the **OSW** directories.  
  `C:\openstudio-3.7.0\bin\openstudio.exe run -w "[LOCAL_PATH]\workflow.osw"`

**Example:**

```ruby
C:\openstudio-3.7.0\bin\openstudio.exe  run -w "C:\Users\sample_user\baselinePRM\test.osw"
```

{{% notice tip %}}
If the openstudio command is not recognized, either add the executable to your environment PATH or add the full path to the `openstudio-3.7.0\bin` directory.
It is advised to avoid spaces in file and directory names. Use quotation marks when specifying long file names or paths with spaces.
{{% /notice %}}

{{% notice tip %}}
The OpenStudio SDK version needs to be **3.7.0** or later.
{{% /notice %}}

{{< line_break >}}

##### **5. Save the Output**

Once the workflow simulation is **completed**, you will find additional files generated in your project directory.

- An output OSW file `out.osw` is written to indicate whether the simulation run was successful or not.

Once the workflow is completed **unsuccessfully (fails)**:

- A `run` directory is generated with a `run.log` file that contains the error message.

Once the workflow is completed **successfully**:

- A `run` directory is generated which contains all input/output files.
- A `reports` directory is created which contains energyplus's standard html report.
- A `generated_files` directory contains all the generated models and simulation information.
- A `user_data` directory contains all user provided data in `json` format.

![Measure Output Structure](/BEM-for-PRM/get_start/os_cli/images/output_files.png?width=800px&align=left&classes=border)

{{<line_break>}}

#### Optional Steps

This instruction is provided for users who want to access the latest features or bug fixes that are not accessible in the latest OpenStudio SDK.

Follow the above steps by replacing Step 2 and 4 as follows:

{{% notice info %}}
The measures selected on this tab will not run until you run your model, unlike the _*Apply Measure Now*_ option.
{{% /notice %}}
