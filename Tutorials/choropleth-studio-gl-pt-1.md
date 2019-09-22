---
title: "Make a choropleth map, part 1: create a style"
description: Upload custom data to Mapbox Studio, add it to a template style, and use filters to style your data.
thumbnail: choroplethStudioGlPt1
level: 1
topics:
- uploads
- map design
- web apps
language:
- No code
prependJs:
  - "import Icon from '@mapbox/mr-ui/icon';"
  - "import AppropriateImage from '../../components/appropriate-image';"
  - "import { PRODUCTION_TOKEN, VERSION_MAPBOXGLJS } from '../../constants'"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
contentType: tutorial
---

使用等值线 [**choropleth**](https://en.wikipedia.org/wiki/Choropleth_map) 是在地图上显示数据分布的一种方法，这是一种基于特定值对区域进行着色的主题地图。在本指南中，您将使用 Mapbox Studio 和 Mapbox GL JS 绘制一张显示美国各州人口密度的地图。（如果您熟悉 [Leaflet](/help/glossary/leaflet) ，这张地图[可能看起来会很熟悉](http://leafletjs.com/examples/choropleth/)！）

{{
  <DemoIframe src="/help/demos/choropleth-studio-gl/demo-five.html" />
}}

## 准备开始

在本指南中，您将使用两个 MapBox 工具：

- [Mapbox Studio](/help/glossary/mapbox-studio) 用于添加数据和创建地图样式。
- [Mapbox GL JS](/help/glossary/mapbox-gl-js) 为地图添加交互并在网页上展示。

## 下载数据

对于本指南，您需要下载一些数据 [**data**](/help/demos/choropleth-studio-gl/stateData.geojson) 。这个 GeoJSON 文件是从Leaflet的等值线教程 [the Leaflet choropleth tutorial](http://leafletjs.com/examples/choropleth/) 引用来的，包含了美国各州人口密度的数据。

## 上传您的数据

为了将人口密度数据添加 Mapbox Studio 的一个样式中，您需要将其上传到您的帐户。请前往 Mapbox Studio 中的 [**Tilesets** page](https://studio.mapbox.com/tilesets) 页面上传您的数据。

在您的 Tilesets 页面上，点击 **New tileset** 按钮。选择 `stateData.geojson` 文件并将其上传到您的帐户。

上传完成后，单击文件名旁边的箭头打开其信息页。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1DataUploadSuccess"
    alt="upload dialog leading to your tileset's information page"
  />
}}

## 检查切片集

当您将矢量数据上传到 Mapbox 帐户时，我们的服务器会将其转换为矢量切片集 [vector tileset](/help/glossary/tileset) ，以便其可以在 Mapbox Studio 样式编辑器和 Mapbox GL JS 中快速高效地呈现。切片集的信息界面展示了一些根据您上传的数据创建的瓦片数据集的有用信息。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1StatedataPage"
    alt="new state data tileset information page"
  />
}}

- 在图的中间，您可以看到原始 GeoJSON 文件的属性： **density** 和 **name** 。上传的切片集保留了原始数据文件的属性，在为数据集添加样式规则时可以使用这些属性。
- **tileset ID** 是此瓦片数据集的唯一标识符。
- **Format**, **Type**, **Size**, and **Bounds** 提供了切片集的基本信息。
- **Zoom extent** 会告知您上传的数据生成切片的缩放级别。不用担心这个数值。矢量切片由矢量数据组成，可以超缩放 [overzoomed](/help/glossary/overzoom/) 并设置为缩放级别22的样式。


## 创建一个新样式

在您检查完数据后，是时候创建一个新样式了以便您可以将其放到地图上！跳转至 [Styles page](https://studio.mapbox.com) 。单击 **New style** 按钮。找到 _Basic Template_ ，然后点击 **Customize Basic Template** 。

太棒了！欢迎使用 Mapbox Studio 样式编辑器。在这里您将创建您的地图样式。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1NoData"
    alt="Mapbox Studio style editor showing default Basic style"
  />
}}

重命名样式以便之后找到它。单击屏幕左上角的标题区域，将标题从 Basic Template 更改为 Population 。

_如果这是您第一次使用样式编辑器，请阅读 [Mapbox Studio Manual](https://www.mapbox.com/studio-manual/reference/styles/) 以了解有关入门的详细信息。_

## 添加一个新图层

要添加人口密度数据并设置其样式，您需要向地图中添加新图层。在图层面板的顶部，点击 **+ Add layer** 。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1NewLayer"
    alt="Mapbox Studio add new layer"
  />
}}

编辑器现在以X射线模式显示地图。X射线模式显示了添加到样式源中的所有数据，不管是否有图层来设置样式。

在 _New layer_ 面板中，查看 `statedata` 源的 _Data sources_ 列表。单击 tileset ，然后选择源图层作为此新样式图层的源。

默认的基本地图视图中心点不是美国。 Mapbox Studio 识别到您上传的数据集中在不同的位置，因此它会显示 _"This tileset isn't available from your map view.（此切片集在您的地图视图中不可用。）"_ 的提示信息。点击 **Go to data** ，然后地图视图将重新聚焦到美国。

您的新图层将在X射线地图上高亮显示。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1StateDataXray"
    alt="Mapbox Studio set data dialog showing x-ray mode"
  />
}}

点击 **Style** 选项卡，地图将切换回显示新图层样式的模式。您将在地图上看到默认样式的州数据（黑色，不透明度为100%）。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1StateDataBlack"
    alt="Mapbox Studio state data with default fill-color style"
  />
}}

您可以通过点击面板顶部的图层名称重命名图层。重命名新图层为 `statedata` 。

## 设置图层样式

在原始的 Leaflet 地图上，数据样式是基于每个州的人口密度来生成的：

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1LeafletChoropleth"
    alt="original Leaflet choropleth map"
  />
}}

### 数据驱动样式

在 Mapbox Studio 样式编辑器中，可以根据每个州的人口密度为其指定颜色。点击 `statedata` 图层的 **Style** l链接。接下来，点击 **Style across data range** 。

在 *Choose a numeric data field* 下，选择 `density` 这个选项，因为您想要根据每个州的人口密度来设置其样式。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1InterpolateFromData"
    alt="Mapbox Studio style editor interpolate from data"
  />
}}

变化率是 _linear_ 。点击 **Edit** ，然后选择 **Step** 。点击 **Done** 。由于已将变化率设置为匀速，因此两个点之间的每个密度范围的颜色都将是统一的。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1DataSetRateChange"
    alt="Mapbox Studio style editor set the rate of change to step"
  />
}}

现在是时候开始添加间隔和颜色了！您将创建几个间隔以将州划分成具有相似人口密度的组。点击第一个密度间隔中的 **Edit** 。根据您上传的数据中的信息，第一个间隔点设定在 _1.264_ 。点击它并将颜色更改为 `#FFEDA0` 。点击 **Done** 。

将下一个间隔点的 *density* 更改为 `10` ，并将颜色更改为 `#FFEDA0` 。点击 **Done** 。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1SetFirstDensityStop"
    alt="Mapbox Studio style editor set first density stop"
  />
}}

点击 **+ Add another stop** 。将 *density* 更改为 `20` ，并将颜色更改为 `#FED976` 。点击 **Done** 。

用同样的方法创建以下另外的间隔点：

- `50`: `#FEB24C`
- `100`: `#FD8D3C`
- `200`: `#FC4E2A`
- `500`: `#E31A1C`
- `1000`: `#BD0026`

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1SetAllDensityStops"
    alt="Mapbox Studio style editor set colors for all density stops"
  />
}}

当您开始添加间隔点时，您将看到右侧地图的变化以反映新的间隔。在这种情况下，人口密度在 `0-10` 之间的所有州将被指定为 `#FFEDA0` 颜色，人口密度在 `10-20` 之间的所有州将被指定为 `#FED976` 颜色，依此类推。

为州添加一个精致的轮廓样式并减少它们的不透明度，以帮助您的读者区分相邻的州。将 *Opacity* 更改为 `0.6` 。接下来，将 *1px stroke* 样式属性更改为 `#FFF` 。

## 重新排列您的图层

在您创建并设置图层样式后，您的地图应如下所示：

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1DataOnTop"
    alt="screenshot of the choropleth map of the United States with custom data on top of place labels"
  />
}}

一切看起来都不错，除了一件事 - 标签在数据图层下面！ Mapbox Studio 样式编辑器最酷的一点就是您可以对地图的任意元素重新排序。这意味着您可以把标签图层放在数据图层的上方。

将鼠标悬停在 `statedata` 图层上。在最左侧的图层列表中点击图层名称旁边的列表符号 {{<Icon name='menu' inline={true} />}} ，然后将图层拖到标签图层下方。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1MoveStatedataLayer"
    alt="Mapbox Studio style editor move statedata layer"
  />
}}

由于人口密度数据中的标签是最重要的，而城市标签则出现在更重要的州标签之上。若要关闭城市标签，请点击 **{{<Icon name='filter' inline={true} />}} Filter layers** 并键入 _place_ 。这将筛选出所有城市标签图层。通过按住 `command` (Mac) 或 `CTRL` (Windows) 同时选中这些图层。接下来，单击图层面板顶部的 {{<Icon name='eye' inline={true} />}} 按钮以关闭这些图层的可见性。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1HideLayers"
    alt="hide layers in the Mapbox Studio style editor"
  />
}}

为了使州之间的边界更明显，您可以将图层列表中的 **admin-state-province** 图层移动到 `statedata` 图层的上方。

{{
  <AppropriateImage
    imageId="choroplethStudioGlPt1MoveLayers"
    alt="move layers in the Mapbox Studio style editor"
  />
}}

## 发布样式

现在您的地图看起来不错，是时候发布了！点击屏幕右上角的 **Publish** 按钮，然后在下一次提示中再次点击 **Publish** 。

哇哦！您的样式现在发布了！如果返回 Styles 界面，您将在列表顶部看到您的新样式。

## 下一步

进入 [part 2](/help/tutorials/choropleth-studio-gl-pt-2/) 学习如何向地图添加交互元素，并使用 Mapbox GL JS 将其发布到 Web 。
