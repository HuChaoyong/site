---
title: ArcGIS_JS_API
catalog: true
date: 2018-10-28 01:15:00
subtitle:
header-img:
tags:
- ArcGIS
- JS
---
All of this is based on ArcGIS JS API 4.x
# Custom Layer
## Custom WMTS request
> like just request level 17 map tile
    ```javascript
    /* 
        Test by API 4.6
       when request level lower than 17, it will request urlTemplate resource
       if level is more than 17 it will request 
       'http://localhost/#'[bad request]
       this situation make you have a high level tile image, then your
       high level request don't have any tile image
    */

    // "esri/request" => esriRequest
    // "esri/layers/BaseTileLayer" => BaseTileLayer
    const TDTLayer = BaseTileLayer.createSubclass({
        properties: {
            urlTemplate: null
        },
        getTileUrl: function(level, row, col) {
            // you can define any request rule here
            if (level < 17) {
                return this.urlTemplate;
            } else {
                return 'http://localhost/#';
            }
        },
        fetchTile: function(level, row, col) {
            const url = this.getTileUrl(level, row, col);
            return esriRequest(url, {
                responseType: 'image',
                allowImageDataAccess: true
            }).then(
                function(response) {
                    const image = response.data;
                    const width = this.tileInfo.size[0];
                    const height = this.tileInfo.size[1];
                    const canvas = document.createElement('canvas');
                    const context = canvas.getContext('2d');
                    canvas.width = width;
                    canvas.height = height;
                    context.drawImage(image, 0, 0, width, height);
                    return canvas;
                }.bind(this)
            );
        }
    });
    // Use like simple layer
    const tdt_img = new TDTLayer({
        urlTemplate: "http://t0.tianditu.com/DataServer?T=vec_w&x={col}&y={row}&l={level}",
        id: "img_layer"
    });
    map.add(tdt_img);
```

