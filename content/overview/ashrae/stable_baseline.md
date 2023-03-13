---
title: "ASHRAE 90.1 Stable Baseline"
date: 2022-09-21T14:48:04-07:00
weight: 112
draft: false
pre: "<b>- </b>"
---

**Appendix G** uses a stable baseline approach with efficiency levels set at values that are not intended to be updated with each new addition of the code. Instead, the proposed building energy performance needs to exceed that of the baseline by an amount commensurate with the code year being evaluated.

> #### Background
> - These changes came about with the modeling procedures in the 2016 standard, that are fundamentally different from previous versions in that the baseline building is fixed to be roughly equal in stringency to Standard 90.1-2004 and compliance is determined through a metric called Performance Cost Index (PCI).
> - Prior to 2016, the Appendix G baseline building stringency changed with each version of Standard 90.1 and sometimes with each addendum that created much confusion for software developers, energy modelers and program administrators. 
> - For many use-cases, an excessive amount of time was spent creating the baseline building and verifying its correctness. With the new procedure, the intent is that the baseline building stringency does not change. 
> - Instead, as the standard becomes more stringent, a greater level of improvement over the stable baseline is required. The 2004 version of the standard has been used for defining the baseline performance. 
> - This version of the standard has been used as a benchmark by ASHRAE and the US DOE for evaluating the stringency of more recent versions of Standard 90.1 and now it is used as a stable baseline for all performance calculations. Each consecutive version of the Standard would update the target PCI without modifying the stringency of the baseline building requirements.

{{< line_break >}}

#### Appendix G Moving Baseline vs. Stable Baseline

The image below compares the Appendix G "moving baseline" and the "stable baseline".

![Stable Baseline vs. Moving Baseline](/BEM-for-PRM/content/overview/ashrae/images/baseline_stable_moving.jpg?width=800px&align=left&classes=border)
