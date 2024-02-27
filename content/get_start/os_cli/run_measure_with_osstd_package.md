---
title: Run measure w/ OSSTD Package
date: 2022-09-21T14:50:40-07:00
weight: 223
draft: false
pre: "<b>- </b>"
---

{{< line_break >}}

#### C. Steps to run the measure with OSSTD Package (Optional)

This instruction is provided for users who want to access the latest features or bug fixes that are not accessible in the latest OpenStudio SDK.

Follow the steps from [Run the measure](../run_the_measure) instruction by replacing the Setup (Step 2) and Execution steps (Step 4) as mentioned below.

<!-- Are all the steps being followed, verify which one won't be followed? -->

{{< line_break >}}

##### **Setup**

Download this [openstudio-standards package](https://github.com/NREL/openstudio-standards/archive/refs/heads/master.zip). Unzip the package to a local directory.

{{% notice tip %}}
Depends on which new feature you'd like to use, the download link could be different. See Github instruction on [how to download source code](https://docs.github.com/en/repositories/working-with-files/using-files/downloading-source-code-archives)
{{% /notice %}}

Follow the [run measure](../run_the_measure) instruction to setup your project folder.

{{< line_break >}}

##### **Execution**

- Open Command Prompt as an administrator.
- Type the following command with paths to the **openstudio\bin**, **openstudio-standard\lib** and the **OSW** directories.  
  `C:\openstudio-3.7.0\bin\openstudio.exe -I "[LOCAL_PATH]\openstudio-standard\lib" run -w "[LOCAL_PATH]\workflow.osw"`

**Example:**

```ruby
C:\openstudio-3.7.0\bin\openstudio.exe  -I "C:\Users\sample_user\openstudio-standard\lib" run -w "C:\Users\sample_user\baselinePRM\test.osw"
```

{{% notice warning %}}
The `-I` command shall point to the local directory of the [Openstudio-standards](https://github.com/NREL/openstudio-standards/archive/refs/heads/master.zip).
{{% /notice %}}

{{% notice warning %}}
It is critical to make sure the OSSTD source code is compatible with or higher than your OpenStudio SDK version. See version matrix in the [Get OpenStudio SDK](../get_openstudio_sdk) page.
{{% /notice %}}
