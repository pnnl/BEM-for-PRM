---
title: "Dev_instruction"
date: 2022-07-26T15:43:57-07:00
draft: false
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
