---
title: Make a heatmap with Mapbox Studio
description: Learn how to add and configure a heatmap layer using Mapbox Studio.
thumbnail: studioHeatmapTutorial
topics:
- web apps
- map design
level: 1
language:
- No code
prependJs:
  - "import * as constants from '../../constants';"
  - "import AppropriateImage from '../../components/appropriate-image';"
  - "import Icon from '@mapbox/mr-ui/icon';"
  - "import { ColorSwatch } from '../../components/color-swatch';"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
  - "import Button from '@mapbox/mr-ui/button';"
contentType: tutorial
---

热力图是一种允许您根据数据属性可视化数据点密度以及点之间的相对差异的数据可视化方式。它们是一种吸引视觉的方式，可以吸引观众浏览地图上显示的数据。

在本教程中，您将使用 Mapbox Studio 创建一个热力图，根据 NASA 收集的数据，显示全球陨石撞击的位置。您还将使用每个陨石的重量（以克为单位）来显示每个陨石的相对大小。


{{
  <DemoIframe src="https://api.mapbox.com/styles/v1/examples/cjikt35x83t1z2rnxpdmjs7y7.html?access_token=MapboxAccessToken#2/11.285/-65.322/0" />
}}

## 准备工作

要完成本教程，您需要：

- **Mapbox 帐户和 access token 。**在 [mapbox.com/signup](https://www.mapbox.com/signup/) 注册一个帐户。您可以在 [Account page](https://account.mapbox.com) 页面上找到 access token 。
- **陨石撞击数据。** 在本教程中，您将使用来自 NASA 开放数据门户 [NASA's open data portal](https://data.nasa.gov/Space-Science/Meteorite-Landings/gh4g-9sfh) 的陨石撞击信息。将此 CSV 文件下载到您的计算机，以便您可以在本教程的第一步中使用它。

{{
<Button href="/help/demos/studio-heatmap/meteorites.csv" passthroughProps={{ download: "meteorites" }} >
    <Icon name='arrow-down' inline={true} /> Download CSV
</Button>
}}

## 上传您的数据

To add the meteorite strike information to a style in Mapbox Studio, you need to upload it to your account first. To upload this file to Mapbox Studio:
要将陨石撞击的信息添加到 Mapbox Studio 的样式，您需要先将其上传到您的帐户。要将此文件上传到 Mapbox Studio：

1. 转到您的 [**Tilesets** page](https://studio.mapbox.com/tilesets) 页面。
2. 单击 **New tileset** 按钮。
3. 导入 `meteorites.csv` 。为此，您可以：
  - 单击 **Select a file** ，然后选择该文件。
  - 或将文件拖到上传位置中。
4. 单击 **Confirm** 将文件上载到您的帐户。

上传完成后，屏幕右下角会出现一个通知图标。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialUploadSuccessful"
    alt="Screenshot showing a successful file upload in Mapbox Studio"
  />
}}

## 创建一种新的样式

现在您已将陨石撞击信息上传为切片集（tileset），您可以将其添加到新的地图样式中。

1. 转到 Mapbox Studio 中的 [Styles page](https://studio.mapbox.com/styles/) 页面。
2. 单击 **New style** 按钮。找到 _Basic Template_ 样式，然后单击 **Customize Basic Template** 。
3. 现在，在 Mapbox Studio 样式编辑器中，您将创建地图样式。重命名您的新样式，以便您以后可以找到它！单击屏幕左上角的标题字段，将标题更改为 _Meteorites_ 。

## 添加热力图图层

要将陨石撞击的数据添加样式并将其设置为热力图图层，您需要在创建的样式中添加新图层。

1. 在图层面板的顶部，单击 **+ Add layer** 。
2. 在 _Data sources_ 列表中，搜索 “meteorites” 并选择 meteorite strike tileset 。
3. 单击 **Type** 选项，然后选择 **Heatmap** 选项。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialLayerType"
    alt="Screenshot showing how to change a new layer type to heatmap in Mapbox Studio"
  />
}}

4. 单击面板顶部的 **Style** 选项卡。
5. 现在陨石撞击切片集是您的样式中的一个热力图图层。单击图层名称并将其更改为 “meteorite strikes” 。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialLayerName"
    alt="Screenshot showing how to change a layer name in Mapbox Studio"
  />
}}

## 设置图层属性
<!-- copyeditor ignore represents -->
Mapbox Studio中的热力图图层具有多个不同的图层属性选项。创建准确展示数据的热力图的关键是了解这些属性选项的作用，并且这有助于掌握显示太多细节和生成大型彩色斑点之间的平衡。

- **颜色（Color）**：定义热力图的颜色渐变，从最小值到最大值。您可以单独调整每个间隔点的密度和颜色，以及添加和删除间隔点。
- **不透明度（Opacity）**：控制热力图图层的全局不透明度。
- **半径（Radius）**：设置每个点的半径（以像素为单位）。随着半径数的增加，热力图结果将变得更平滑，忽略一些细节。
- **权重（Weight）**：测量每个点对热力图外观的影响程度。默认情况下，热力图图层的权重为1，这意味着所有点的权重均等。将权重属性增加到5与将5个点放在同一位置具有相同的效果。您可以使用 _Style across data range_ （样式跨数据范围）和 _Style with data conditions_ （样式与数据条件选项）来根据指定的属性设置点的权重。
- **强度（Intensity）**：权重属性之上的乘数。强度属性主要用来根据缩放级别调整热力图外观。

### 半径

在热力图中，点的半径应随着缩放级别的增加而增加，以便在点变得更加分散时保持热力图的平滑度。对于本教程，您将更改默认值（所有缩放级别的半径为 `30 px` ），以便在放大地图时半径增加。

1. 选择热力图图层中的 **Radius** 选项卡。
2. 选择 **Style across zoom range** 。
3. 对于第一个间隔点，将缩放级别为 `0` （缩小到最大程度）时的半径更改为 `5 px` 。
4. 单击 **Done** 。
5. The next stop is already set at zoom level `22` (zoomed in to the maximum extent) with a radius of `30 px`. Leave this stop with these settings.
5. 下一个间隔位置已设置为缩放级别 `22` （放大到最大程度），半径为 `30 px` 。使这个间隔保留这些设置。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialRadius"
    alt="Screenshot showing how to adjust heatmap data point radius in Mapbox Studio"
  />
}}

### 权重

在您上传的切片集中， `mass(g)` 属性的范围为1-60000000克。这是各种各样的陨石尺寸范围。通过设置随着质量增加而增加点权重的挡块，使热力图中大陨石的权重更大。

1. 选择 **Weight** 选项卡。
2. 单击 **Style across data range** 。
3. 系统将提示您选择一个数字数据字段作为该属性样式的依据。选择 `mass(g)` 。<br><br>

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialDataRange"
    alt="Screenshot showing how to choose a data field in the style across data range option"
  />
}}

4. 第一个间隔点已被设置为质量 `0` ，权重为 `1` 。保持这些设置。
5. 下一个间隔点已被设置为数据中的最大质量。将权重更改为 `25` 。
6. 单击 `Done` 。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialWeight"
    alt="Screenshot showing how to adjust heatmap weight in Mapbox Studio"
  />
}}

### 强度

当地图放大时，热力图图层的强度可以增加，以在整个地图范围内保持类似的外观。

1. 选择图层的 **Intensity** 选项卡。
2. 单击 **Style across zoom range** 。
3. 第一个间隔点已被设置为缩放级别为 `0` ，强度为 `1` 。保持这些设置。
4. 将第二个间隔点更改为缩放级别 `22` ，强度为 `10` 。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialIntensity"
    alt="Screenshot showing how to adjust heatmap intensity in Mapbox Studio"
  />
}}

这些图像显示了强度对您的样式外观的影响。右侧的图像显示使用默认值 `1` 的强度，而左侧的图像显示随着缩放级别增加而增加的强度。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialIntensityComparison"
    alt="Screenshot showing the difference between default and custom heatmap intensity in Mapbox Studio"
  />
}}

### 颜色
Mapbox Studio 中的热力图图层带有一组鲜艳的预设颜色，用于表示数据点的密度。但是，对于本教程，我们将更改颜色方案。我们从色彩推荐网站上采用了这些颜色，专门用于制图， [Color Brewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3) 。

1. 选择热力图图层中的 **Color** 选项卡。
2. 第一个间隔点设置的密度为 `0` ，不透明度为 `0` 。如果没有这些设置，陨石图层将是不透明的，会遮挡地图的其余部分。保留这些设置。
3. 单击第二个间隔点，其密度为 `0.1` 。将颜色更改为 {{<ColorSwatch color="#ffffb2" />}} 。
4. 单击 **Done** 。
5. 使用以下颜色调整剩余密度间隔点：
  - `0.3`: {{<ColorSwatch color="#feb24c" />}}
  - `0.5`: {{<ColorSwatch color="#fd8d3c" />}}
  - `0.7`: {{<ColorSwatch color="#fc4e2a" />}}
  - `1.0`: {{<ColorSwatch color="#e31a1c" />}}

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialColor"
    alt="Screenshot showing how to change heatmap stop colors in Mapbox Studio"
  />
}}

### 不透明度
您的热力图看起来不错，但由于陨石撞击点互相重合，因此很难读取位置标签。调整热力图图层的不透明度，以便您可以读取标签：

1. 选择 **Opacity** 图层。
2. 通过使用滑块或直接键入数字，将不透明度更改为 `0.7` 。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialOpacity"
    alt="Screenshot showing how to change heatmap layer opacity in Mapbox Studio"
  />
}}

## 发布

当您完成地图样式的编辑后，通过单击屏幕右上角的 **Publish** 来发布更改。单击 **Publish** 按钮时，窗口将显示此样式的上一个版本和当前版本之间的差异。如果您对更改感到满意，请单击 **Publish** 。您创建的样式现在可以从各种工具和应用程序中共享。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialPublish"
    alt="Screenshot showing how to change heatmap stop colors in Mapbox Studio"
  />
}}

如果您返回 Styles 页面，您将在列表顶部看到新样式。

## 完成

You have created a style that displays meteorite strike data from around the world, in a way that also allows you to show the relative mass of each meteorite.
您创建了一种显示世界各地的陨石撞击数据的风格样式，同时还允许您显示每个陨石的相对质量。

{{
  <DemoIframe src="https://api.mapbox.com/styles/v1/examples/cjikt35x83t1z2rnxpdmjs7y7.html?access_token=MapboxAccessToken#2/11.285/-65.322/0" />
}}


## 下一步

Mapbox Studio 为您提供了多种方法使用您创建的新地图样式。您可以直接在网站、 Web 或移动应用程序中使用此地图。请查看 Mapbox Studio 手册的 [Publish style section](https://docs.mapbox.com/studio-manual/overview/publish-your-style/) 部分，了解使用风格样式的不同方法。
