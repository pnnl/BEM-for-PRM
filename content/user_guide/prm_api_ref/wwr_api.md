---
title: "Window API"
date: 2022-09-21T15:06:59-07:00
weight: 332
draft: false
pre: "<b>- </b>"
---

- [Quick use](#quick-use)
- [Function workflow](#function-workflow)

{{< line_break >}}

#### Quick Use

```ruby
climate_zone = "ASHRAE 169-2013-4A"
wwr_building_type = "Office <= 5,000 sq ft"

standard = Standard.build("90.1-PRM-2019")
result, wwr_info = standard.model_apply_prm_baseline_window_to_wall_ratio(model, climate_zone, wwr_building_type)
```

The above code adjust the window to wall ratio in the model to the specified building type. Each building type maps to a window to wall ratio.

| Building Area Types                 | Baseline building vertical fenestration percentage of gross-above-grade wall area |
| ----------------------------------- | --------------------------------------------------------------------------------- |
| `Grocery store`                     | 7%                                                                                |
| `Warehouse (nonrefrigerated)`       | 6%                                                                                |
| `School (secondary and university)` | 22%                                                                               |
| `School (primary)`                  | 22%                                                                               |
| `Retail (strip mall)`               | 20%                                                                               |
| `Retail (stand alone)`              | 11%                                                                               |
| `Restaurant (quick service)`        | 34%                                                                               |
| `Restaurant (full serivce)`         | 24%                                                                               |
| `Office > 50,000 sq ft`             | 40%                                                                               |
| `Office <= 5,000 sq ft`             | 19%                                                                               |
| `Office 5,00 to 50,000 sq ft`       | 31%                                                                               |
| `Hotel/motel > 75 rooms`            | 34%                                                                               |
| `Hotel/motel <= 75 rooms`           | 24%                                                                               |
| `Hospital`                          | 27%                                                                               |
| `Healthcare (outpatient)`           | 21%                                                                               |
| `All other`                         | 40%                                                                               |

The mapping is shown in the above table. Once the function identifies the correct building area, it will applies the window to wall ratio to the entire model.

{{< line_break >}}

#### Function workflow

- ##### Step 1: Check proposed model

  {{<mermaid align="center">}}
  graph LR;
  A(Model) -->|Loop| B(Spaces)
  B -->|Apply building area type| C(BAT)
  B -->|Surface conditioning category| D{Condition}
  D -->|Conditioned space| E(Surfaces)
  C -->|Target WWR| F(Adjust Ratio)
  E -->|Existing WWR| F(Adjust WWR)
  {{</mermaid>}}
