---
title: Arcgis Server Mensuration 3D Publish
catalog: true
date: 2020-07-05 16:53:52
subtitle: Public Image Server which support Mensuration 3D
header-img:
tags:
- ArcGIS
catagories:
- ArcGIS, ArcGISServer
---

# Before Publish

* you will download the DEM data.

# Publish

1. right click your dem data.  select "Share As Image Service"

2. then next window "Publish a service"

3. then next window, select one server site connection, and input the service name.

4. then next window ,select your folder or create one.

5. then next window. this is a setting page. on the left menu, find "Parameters => Mensuration".
    >you will see a "Elevation Source" input bar.<br>
    >A. if your server is installed on the Windows System, open "Elevation Source", select the dem data.<br>
    >B. if your server is installed on the Linux System, before now, you will publish a DEM server.(then now open "Elevation Source", select published DEM server).

6. click  "Publish" on the right top position.

7. after publish success. open you server url.

8. find "Mensuration Capabilities" option, if it's show "Basic,3D"， mean publish is ok.

## How to Mensuration 3d.

* open the rest api for found.
