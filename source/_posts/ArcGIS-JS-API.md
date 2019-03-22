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
   ```
   ```javascript
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


# Query Task By Per Page

> Usually query feature, just return 1000 features, if you want to get more,<br>
you should change server publish config. but it's not Geek!<br>
so query it by per page is a good idea !<br>
because arcgis js api query not support ,but server rest full api support!
```javascript
require([
  "esri/request",
  "esri/Graphic",
  "esri/geometry/SpatialReference",
  "esri/tasks/support/Query",
  "esri/tasks/QueryTask",
  "esri/tasks/support/StatisticDefinition"
], function (esriRequest, Graphic, SpatialReference, Query, QueryTask, StatisticDefinition) {

  const queryPageTool = {
    /**
     * 分页分批请求数据
     * @param {*} query 仅支持属性： where, returnGeometry, outSpatialReference, outFields, orderByFields
     * @param {*} queryTask 
     * @param {*} pageSize 每页请求的数量（一般不大于1000）
     * @param {*} anyField 任意一个字段名
     * @param {*} theadCount 每批请求的个数 
     */
    queryByPage: function (query, queryTask, pageSize, anyField, theadCount) {
      const countStatistic = {
        onStatisticField: anyField,
        outStatisticFieldName: 'count',
        statisticType: 'count'
      };
      const saveArr = [];
      this.queryFeatures(query, queryTask, [countStatistic]).then((countRes) => {
        if (!isNullOrUndefined(countRes.features) && Array.isArray(countRes.features) && countRes.features.length > 0) {
          const recordCount = countRes.features[0].attributes.count;
          console.log('count: ', recordCount);
          this.queryRecycle(query, queryTask, pageSize, 0, recordCount, [], theadCount);
        }
      });
    },
    /**
     * Convert query param to url property
     * @param {*} query 
     * @param  pageSize 单页数量大小
     * @param number firstRecordRid 该页第一条记录的序号
     */
    covertQueryToQueryObj: function (query, pageSize, firstRecordRid) {
      const baseObj = {
        f: 'json',
        spatialRel: 'esriSpatialRelIntersects',
        resultRecordCount: pageSize,
        resultOffset: firstRecordRid
      };
      // 把where转过去
      if (!isNullOrUndefined(query.where)) {
        baseObj['where'] = query.where;
      }

      if (!isNullOrUndefined(query.returnGeometry)) {
        baseObj['returnGeometry'] = query.returnGeometry;
      }

      // 输出坐标系
      if (!isNullOrUndefined(query.outSpatialReference)) {
        baseObj['outSR'] = query.outSpatialReference.wkid;
      }

      // Geometry 转换
      if (!isNullOrUndefined(query.geometry)) {
        baseObj['inSR'] = query.geometry.spatialReference.wkid;
        if (query.geometry.type === 'extent') {
          baseObj['geometry'] = JSON.stringify(query.geometry.toJSON());
          baseObj['geometryType'] = 'esriGeometryEnvelope';
        }
        if (query.geometry.type === 'polygon') {
          baseObj['geometry'] = JSON.stringify(query.geometry.toJSON());
          baseObj['geometryType'] = 'esriGeometryPolygon';
        }
        if (query.geometry.type === 'polyline') {
          baseObj['geometry'] = JSON.stringify(query.geometry.toJSON());
          baseObj['geometryType'] = 'esriGeometryPolyline';
        }
        if (query.geometry.type === 'point') {
          baseObj['geometry'] = JSON.stringify(query.geometry.toJSON());
          baseObj['geometryType'] = 'esriGeometryPoint';
        }
      }

      // 返回字段处理
      if (!isNullOrUndefined(query.outFields) && Array.isArray(query.outFields) && query.outFields.length > 0) {
        baseObj['outFields'] = query.outFields.toString();
      }

      // 排序处理
      if (!isNullOrUndefined(query.orderByFields) && Array.isArray(query.orderByFields)) {
        // baseObj['orderByFields']
        let t = '';
        query.orderByFields.forEach((orderField) => {
          const deal = orderField.trim().replace(/\s+/g, ' '); // 前后去空格并去掉中间重复的空格
          const splitArr = deal.split(' ');
          if (splitArr.length === 1) {
            if (t === '') {
              t += splitArr[0];
            } else {
              t += '%2C' + splitArr[0];
            }
          } else if (splitArr.length === 2 && (splitArr[1] === 'DESC' || splitArr[1] === 'ASC')) {
            if (t === '') {
              t += deal;
            } else {
              t += `%2C${deal}`;
            }
          }
        });
        if (t !== '') {
          baseObj['orderByFields'] = t;
        }
      }
      return baseObj;
    },
    /**
     * 将传入的feature信息转换成 Graphic
     * @param object : feature 请求回来的Feature 
     * @param string: geometryType 图形类型
     * @param spatialReference: sr 返回的空间参考对象
     * @param Graphic: Graphic 
     * @param SpatialReference: SpatialReference class
     */
    convertFeatureToGraphic: function (feature, geometryType, sr, Graphic, SpatialReference) {
      if (isNullOrUndefined(feature.geometry)) {
        return feature;
      }
      const spatialReference = new SpatialReference(sr);
      if (geometryType.toLowerCase().includes('polygon')) {
        feature.geometry.type = 'polygon';
      } else if (geometryType.toLowerCase().includes('polyline')) {
        feature.geometry.type = 'polyline';
      } else if (geometryType.toLowerCase().includes('point')) {
        feature.geometry.type = 'point';
      }
      feature['spatialReference'] = spatialReference;
      const g = new Graphic(feature);
      return g;
    },
    /**
     * 将查询对象转成字符串
     * @param object: newQuery 查询对象
     */
    convertQueryToRequestString: function (newQuery) {
      let body = '';
      for (const prop in newQuery) {
        if (!isNullOrUndefined(newQuery[prop])) {
          body += (encodeURIComponent(prop) + '=' + encodeURIComponent(newQuery[prop])) + '&';
        }
      }
      body = body.replace(/\&$/g, '');
      return body;
    },
    queryRecycle: function (query, queryTask, pageSize, firstRecordRid, recordCount, features, theadCount) {
      if (features.length < recordCount) {
        const promiseArr = [];
        const max = firstRecordRid + theadCount * pageSize;
        for (; firstRecordRid < max && firstRecordRid < recordCount; firstRecordRid += pageSize) {
          promiseArr.push(this.queryFeaturesByPage(query, queryTask, pageSize, firstRecordRid));
        }
        Promise.all(promiseArr).then((resArr) => {
          resArr.forEach((subRes) => {
            if (!isNullOrUndefined(subRes) && !isNullOrUndefined(subRes.features)) {
              features = features.concat(subRes.features);
            }
          });
          this.queryRecycle(query, queryTask, pageSize, firstRecordRid, recordCount, features, theadCount);
        }).catch((e) => {
          console.error(e);
          observer.error(e);
        });
      } else {
          // output all record
        console.log('all records： ', features);
      }
    },
    queryFeatures: function (query, queryTask, constraints) {
      const q = new Query(query);
      const qt = new QueryTask(queryTask);
      if (!isNullOrUndefined(constraints) && Array.isArray(constraints)) {
        q.outStatistics = [];
        constraints.forEach(item => {
          const tempStatisticDefinition = new StatisticDefinition();
          tempStatisticDefinition.statisticType = item.statisticType;
          tempStatisticDefinition.onStatisticField = item.onStatisticField;
          tempStatisticDefinition.outStatisticFieldName = item.outStatisticFieldName;
          q.outStatistics.push(tempStatisticDefinition);
        });
      }
      return qt.execute(q);
    },
    queryFeaturesByPage: function (query, queryTask, pageSize, firstRecordRid) {
      const newQuery = this.covertQueryToQueryObj(query, pageSize, firstRecordRid);
      const body = this.convertQueryToRequestString(newQuery);
      return esriRequest(`${queryTask.url}/query?${body}`).then((res) => {
        if (!isNullOrUndefined(res.data) && !isNullOrUndefined(res.data.features) && Array.isArray(res.data.features)) {
          res.data.features.forEach((feature, idx) => {
            res.data.features[idx] = this.convertFeatureToGraphic(feature, res.data.geometryType, res.data.spatialReference, Graphic, SpatialReference);
          });
          return res.data;
        } else {
          return null;
        }
      });
    }
  };



  // test
  const query = {
    where: "1=1",
    outFields: ['*'],
    returnGeometry: true
  };
  const queryTask = { url: 'http://localhost:6080/arcgis/rest/services/test/temp/MapServer/0' };
  // every cycle will request 5 times, every time request 1000 features.
  queryPageTool.queryByPage(query, queryTask, 1000, 'objectid', 5);
});
function isNullOrUndefined(obj) {
  return obj === undefined || obj === null;
}
```



