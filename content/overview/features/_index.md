+++
title = "Features"
date = 2022-09-21T14:49:22-07:00
weight = 12
chapter = true
+++

### Current Features of the OSSTD-PRM

{{< line_break >}}

Here are some key features of the OSSTD-PRM:

- Automating the 90.1 baseline generation that can help to addres accuracy and tuning of the models.
- Flexibility for the baseline generation.
- Model system transformation instead of model transformations.
- API calls for specific systems without running through the entire baseline generation
- With 'rule-set' specific data not typically found in the EnergyPlus model (space), mixed typologies can be adressed through the use of compliance datasets that are included in our user documentation.
- There are currently 16 .csv files in the ‘compliance data’ set, each covering a specific set of /ASHRAE 90.1-2019 compliance data to OpenStudio object.
- The tool includes ‘compliance data’ as a set of .csv files with fixed format to improve accuracy of the PRM model. This data can be used in the ASHRAE 90.1-2019 PRM method to provide additional information that cannot be found in the OpenStudio Model (e.g., motor power).
