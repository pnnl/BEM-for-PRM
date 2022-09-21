---
title: "Dev_instruction"
date: 2022-07-26T15:43:57-07:00
weight: 41
draft: false
pre: "<b>- </b>"
---

## Image

Images uses relative links to the local images folder (in the same level as the markdown file). A preceding exclamation point is needed.

```md
![PNNL Campus](/BEM-for-PRM/basics/images/PNNL_campus.jpg)
```

![PNNL Campus](/BEM-for-PRM/basics/images/PNNL_campus.jpg)

You can adjust the image size by using the css width and/or height to the link image.

```md
![PNNL Campus](/BEM-for-PRM/basics/images/PNNL_campus.jpg?width=200px)
```

![PNNL Campus](/BEM-for-PRM/basics/images/PNNL_campus.jpg?width=200px)

And add CSS (border) to it

```md
![PNNL Campus](/BEM-for-PRM/basics/images/PNNL_campus.jpg?width=200px&classes=border)
```

![PNNL Campus](/BEM-for-PRM/basics/images/PNNL_campus.jpg?width=200px&classes=border)

## Tips/Notes/Warnings

### Note

{{% notice note %}}
A notice disclaimer
{{% /notice %}}

### Code

```ruby
  # Template method for adding a setpoint manager for a coil control logic to a heating coil.
  # ASHRAE 90.1-2019 Appendix G.
  #
  # @param model [OpenStudio::Model::Model] Openstudio model
  # @param thermalZones Array([OpenStudio::Model::ThermalZone]) thermal zone array
  # @param coil Heating Coils
  # @return [Boolean] true
  def model_set_central_preheat_coil_spm(model, thermalZones, coil)
    return true
  end
```
