---
layout: post
title: 百度地图API的简单使用
categories: API
description: 百度地图API的简单使用
keywords: API
---

首先编写HTML页面的基础代码,并引入API文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>BMap</title>
    <!-- 百度地图API js文件  ak=密钥 -->
    <script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=Z6VLb33wYXOcBcWtpVMjPvbU0DpUdQup"></script>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

##### 然后初始化地图等一些逻辑操作

1.话不多说，直接上代码

```js
    var map = {}

    // 列表请求 https://www.easy-mock.com模拟数据
    var requestList = ()=>{
        axios.ajax({
            url:'https://www.easy-mock.com/mock/5a7278e28d0c633b9c4adbd7/api/map/bike_list',
            data:{
                params:params
            }
        }).then((res)=>{
            if(res){
                renderMap(res.result);
            }
        })
    }


    // 渲染地图
    function renderMap(res) {
        var list = res.route_list;
        map = new window.BMap.Map("root", {enableMapClick: false});
        var gps1 = list[0].split(',');
        var startPoint = new window.BMap.Point(gps1[0], gps1[1]);
        var gps2 = list[list.length - 1].split(',');
        var endPoint = new window.BMap.Point(gps2[0], gps2[1]);

        map.centerAndZoom(endPoint, 11);
        // map.clearOverlays();

        //添加起始图标
        var startPointIcon = new window.BMap.Icon("/assets/start_point.png", new window.BMap.Size(36, 42), {
            imageSize: new window.BMap.Size(36, 42),
            anchor: new window.BMap.Size(18, 42)
        });
        
        var bikeMarkerStart = new window.BMap.Marker(startPoint, { icon: startPointIcon });
        map.addOverlay(bikeMarkerStart);

        var endPointIcon = new window.BMap.Icon("/assets/end_point.png", new window.BMap.Size(36, 42), {
            imageSize: new window.BMap.Size(36, 42),
            anchor: new window.BMap.Size(18, 42)
        });
        var bikeMarkerEnd = new window.BMap.Marker(endPoint, { icon: endPointIcon });
        map.addOverlay(bikeMarkerEnd);

        var routeList = [];
        list.forEach((item)=>{
            var p = item.split(",");
            var point = new window.BMap.Point(p[0], p[1]);
            routeList.push(point);
        })
        // 行驶路线
        var polyLine = new window.BMap.Polyline(routeList, {
            strokeColor: "#ef4136",
            strokeWeight: 3,
            strokeOpacity: 1
        });
        map.addOverlay(polyLine);

        // 服务区路线
        var serviceList = res.service_list;
        var servicePointist = [];
        serviceList.forEach((item) => {
            var point = new window.BMap.Point(item.lon, item.lat);
            servicePointist.push(point);
        })
        // 画线
        var polyServiceLine = new window.BMap.Polyline(servicePointist, {
            strokeColor: "#ef4136",
            strokeWeight: 3,
            strokeOpacity: 1
        });
        map.addOverlay(polyServiceLine);

        // 添加地图中的自行车
        var bikeList = res.bike_list;
        var bikeIcon = new window.BMap.Icon("/assets/bike.jpg", new window.BMap.Size(36, 42), {
            imageSize: new window.BMap.Size(36, 42),
            anchor: new window.BMap.Size(18, 42)
        });
        bikeList.forEach((item) => {
            var p = item.split(",");
            var point = new window.BMap.Point(p[0], p[1]);
            var bikeMarker = new window.BMap.Marker(point, { icon: bikeIcon });
            map.addOverlay(bikeMarker);
        })
        
        // 添加地图控件
        addMapControl();
    };

    // 添加地图控件
    function addMapControl() {
        var map = map;
        // 左上角，添加比例尺
        var top_right_control = new window.BMap.ScaleControl({anchor: window.BMAP_ANCHOR_TOP_RIGHT});
        var top_right_navigation = new window.BMap.NavigationControl({anchor: window.BMAP_ANCHOR_TOP_RIGHT});
        //添加控件和比例尺
        map.addControl(top_right_control);
        map.addControl(top_right_navigation);
        map.enableScrollWheelZoom(true);
        // legend.addLegend(map);
    };
```


