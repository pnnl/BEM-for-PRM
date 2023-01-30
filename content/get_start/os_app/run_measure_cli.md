---
title: Run measure in CLI
date: 2022-09-21T14:50:40-07:00
weight: 214
draft: false
pre: "<b>- </b>"
---

#### Introduction

This guide is intended to provide instructions on how to run the _Create ASHRAE 90.1-2019 PRM Model_ measure via the [command line interface (CLI)](http://nrel.github.io/OpenStudio-user-documentation/reference/command_line_interface/).

{{< line_break >}}

#### Background

The _Create ASHRAE 90.1-2019 PRM Model_ measure provides a tool to automatically generate the baseline model according to the rules of the 2019 version of the ASHRAE 90.1 Appendix G Performance Rating Method.

{{< line_break >}}

#### Usage Instructions

##### Setup

To get started :

1. Download and install [OpenStudio v3.5.0](https://github.com/NREL/OpenStudio/releases/tag/v3.5.0) using the default components selection.
2. Download this [openstudio-standards package](https://github.com/NREL/openstudio-standards/archive/refs/heads/master.zip). Unzip the package to a local directory.
3. Setup a local directory to include all required inputs (the following example is one way to setup the directory):

- **Seed mode**: place the `.osm` file in a directory named `files`. The Openstudio model should meet the [model requirements](https://pnnl.github.io/BEM-for-PRM/user_guide/model_requirements/).
- **Weather file**: place the `.epw` file in a directory named `weather`.
- **Measure**: place the `CreateBaselineBuilding` measure in a directory called `measures`.
- **OS Workflow**: place the `.osw` file in the main directory.
- **User Data (_Optional_)**: place the `.csv` file in the main directory (same level as the `.osw`). The use of the user data can be found in the [add compliance data section](https://pnnl.github.io/BEM-for-PRM/user_guide/add_compliance_data/).

The image below shows one way to setup the main directory and their correponding sub-directories.

![Directory Setup](/BEM-for-PRM/get_start/os_app/images/DirectSetup.JPG?width=800px&align=left&classes=border)

The OSW file can be structured as shown in the image below.

- Lines 2-3 define the seed model and weather file.
- Lines 4-5 define the measure path.
- Lines 9-14 contain arguments to be passed to the measure. A detail description of the arguments can be found in the [API document](https://pnnl.github.io/BEM-for-PRM/user_guide/prm_api_ref/baseline_generation_api/).

![OS Workflow Structure](/BEM-for-PRM/get_start/os_app/images/osw.JPG?width=600px&align=left&classes=border)

{{% notice tip %}}
**Importnat Note:**
The directory names are not case sensitive. If you want to change the names of the directories that contain the seed model and weather file, make sure to define the paths in the OSW file.
{{% /notice %}}

{{< line_break >}}

#### Execution

- Open Command Prompt as an adminstrator.
- Type the following command with paths to the **openstudio\bin**, **openstudio-standard\lib** and the **OSW** directories.  
  `C:\openstudio-3.5.0\bin\openstudio.exe -I "[LOCAL_PATH]\openstudio-standard\lib" run -w "[LOCAL_PATH]\workflow.osw"`

**Example:**

```ruby
C:\openstudio-3.5.0\bin\openstudio.exe  -I "C:\Users\example_user\openstudio-standard\lib" run -w "C:\Users\example_user\baselinePRM\test.osw"
```

{{% notice warning %}}
The `-I` command shall pointing to the local directory of the [Openstudio-standards](https://github.com/NREL/openstudio-standards/archive/refs/heads/master.zip).
{{% /notice %}}

{{% notice tip %}}
If the openstudio command is not recognized, either add the executable to your environemnt PATH or add the full length to the `openstudio-3.5.0\bin` directory.
It is advised to avoid spaces in file and directory names. Use quotation marks when specifying long file names or paths with spaces.
{{% /notice %}}

{{% notice tip %}}
The OpenStudio SDK version can be **3.2.1** or later.
{{% /notice %}}

{{< line_break >}}

#### Output

Once the workflow is completed:

- An output OSW file `out.osw` is written to indicate whether or not the simulation run was successful.

Once the workflow is completed **successfully**:

- A `run` directory is generated which contains all input/output files.
- A `reports` directory is created which contains energyplus's standard html report.
