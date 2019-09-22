---
title: Build indoor floorplans with Mapbox
description: Upload a floorplan to create an indoor map.
thumbnail: indoorFloorplans
level: 3
topics:
- uploads
- map design
- third party integration
language:
- Varies
prereq: Familiarity with GIS.
contentType: tutorial
---

您可以通过使用 Mapbox 在地图中放入楼层平面图或者无地理信息的数据。有多种方法可以实现这类需求。关键点在于，当您的数据应用于地图应用时，需要给其一些地理位置参考。地理位置参考根据不同的数据类型分为以下几条：

- 如果您需要在平面中叠加一张图片，您需要将图片地理配准，请点击 [georeference]（https://docs.mapbox.com/help/tutorials/georeferencing-imagery/） 。
- 如果您想要使用类似 [Mapbox.js](https://www.mapbox.com/mapbox.js/) 的工具使您的数据像切片图块一样显示，请阅读这条将非地图图像转为地图图像的说明 [this guide]（http://www.macwright.org/2012/08/13/images-as-maps.html） 。
- 在已知图片边界信息的情况下，您可以参照 [this example]（https://www.mapbox.com/mapbox.js/example/v1.0.0/imageoverlay-georeferenced/） 使用 Leaflet 中的 `imageOverlay` constructor 来完成。
- 如果您的数据为地理矢量文件格式，比如 GeoJSON 、 KML ，或者 Shapefile ，您可以将这类数据作为 [tileset](https://docs.mapbox.com/help/glossary/tileset/) 上传到 [Mapbox Studio](https://github.com/mapbox/mapbox-studio-classic) ，将其添加到一个 [style](https://www.mapbox.com/studio-manual/reference/styles/) ，并使用 Mapbox GL JS 将其嵌入您的网站。
- 如果您正在使用 Mapbox GL JS，可以参照 [this example]（https://www.mapbox.com/mapbox-gl-js/example/3d-extrusion-floorplan/） 使用`fill-extrusions`创建一个三维的楼层平面。
