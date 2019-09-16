---
title: Make a heatmap with Mapbox Studio
title: 使用 Mapbox 制作热力图
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

A heatmap is a way of representing data that allows you to visualize both data point density as well as the relative differences between points based on data properties. They are a visually engaging way to encourage your audience to explore the data displayed on your map.
热力图是一种数据可视化允许您根据数据属性可视化数据点密度以及点之间的相对差异的方式。它们具有一种吸引视觉的方式，可以吸引观众浏览地图上显示的数据。

In this tutorial, you will use Mapbox Studio to create a heatmap that displays the location of meteorite strikes around the world, based on data collected by NASA. You will also use the weight of each meteorite in grams to display the relative size of each meteorite.
在本教程中，您将使用Mapbox Studio创建一个热力图，根据NASA收集的关于全球陨石撞击的位置数据。您还将使用每个陨石的重量（以克为单位）来显示每个陨石的相对大小。


{{
  <DemoIframe src="https://api.mapbox.com/styles/v1/examples/cjikt35x83t1z2rnxpdmjs7y7.html?access_token=MapboxAccessToken#2/11.285/-65.322/0" />
}}

## Getting started
## 准备工作

To complete this tutorial, you will need:
要完成本教程，您需要：

- **A Mapbox account and access token.** Sign up for an account at [mapbox.com/signup](https://www.mapbox.com/signup/). You can find your access tokens on your [Account page](https://account.mapbox.com).
- **Mapbox 帐户和 access token.** 在 [mapbox.com/signup](https://www.mapbox.com/signup/). 注册一个帐户。您可以在 [Account page](https://account.mapbox.com).页面上找到access token。
- **Meteorite strike data.** In this tutorial, you will be using information about meteorite strikes from [NASA's open data portal](https://data.nasa.gov/Space-Science/Meteorite-Landings/gh4g-9sfh). Download this CSV file to your computer so that you can access it in the first step of the tutorial.
- **陨石撞击数据.** 在本教程中，您将使用来自NASA开放数据门户 [NASA's open data portal](https://data.nasa.gov/Space-Science/Meteorite-Landings/gh4g-9sfh). 的陨石撞击信息。将此CSV文件下载到您的计算机，以便您可以在本教程的第一步中使用它。

{{
<Button href="/help/demos/studio-heatmap/meteorites.csv" passthroughProps={{ download: "meteorites" }} >
    <Icon name='arrow-down' inline={true} /> Download CSV
</Button>
}}

## Upload your data
## 上传您的数据

To add the meteorite strike information to a style in Mapbox Studio, you need to upload it to your account first. To upload this file to Mapbox Studio:
要将陨石撞击的信息添加到Mapbox Studio中的样式，您需要先将其上传到您的帐户。要将此文件上传到 Mapbox Studio：

1. Go to your [**Tilesets** page](https://studio.mapbox.com/tilesets).
1. 转到您的 [**Tilesets** page](https://studio.mapbox.com/tilesets).页面。
2. Click the **New tileset** button.
2. 单击 **New tileset** 按钮。
3. Import `meteorites.csv`. To do so, you can either:
3. 导入`meteorites.csv`。为此，您可以：
  - Click **Select a file** and select the file.
  - 单击 **Select a file** ，然后选择该文件。
  - Drag the file into the uploader.
  - 将文件拖到上传位置中。
4. Click **Confirm** to upload the file to your account.
4. 单击 **Confirm** 将文件上载到您的帐户。

When the upload is complete, there will be a notification icon in the lower right corner of the screen.
上传完成后，屏幕右下角会出现一个通知图标。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialUploadSuccessful"
    alt="Screenshot showing a successful file upload in Mapbox Studio"
  />
}}

## Create a new style
## 创建一种新的风格

Now that you have uploaded the meteorite strike information as a tileset, you can add it to a new map style.
现在您已将陨石撞击信息上传为 tileset，您可以将其添加到新的地图样式中。

1. Go to your [Styles page](https://studio.mapbox.com/styles/) in Mapbox Studio.
1. 转到 Mapbox Studio 中的 [Styles page](https://studio.mapbox.com/styles/) 页面。
2. Click the **New style** button. Find the _Basic Template_ style and click **Customize Basic Template**.
2. 单击 **New style** 按钮。找到 _Basic Template_ 样式，然后单击 **Customize Basic Template**。
3. Now in the Mapbox Studio style editor, you will create your map style. Rename your new style so that you can find it later! Click into the title field in the upper left side of the screen and change the title to _Meteorites_.
3. 现在，在Mapbox Studio样式编辑器中，您将创建地图样式。重命名您的新风格，以便您以后可以找到它！单击屏幕左上角的标题字段，将标题更改为 _Meteorites_。

## Add a heatmap layer
## 添加热力图图层

To add and style the meteorite strike data as a heatmap layer, you will need to add a new layer to the style you created.
要将陨石撞击的数据添加样式并将其设置为热力图图层，您需要在创建的样式中添加新图层。

1. At the top of the layer panel, click **+ Add layer**.
1. 在图层面板的顶部，单击 **+ Add layer**。
2. In the list of _Data sources_, search for "meteorites" and select the meteorite strike tileset.
2. 在 _Data sources_列表中，搜索“meteorites”并选择 meteorite strike tileset。
3. Click the **Type** option, and select the **Heatmap** option.
3. 单击“类型”选项，然后选择 **Heatmap** 选项。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialLayerType"
    alt="Screenshot showing how to change a new layer type to heatmap in Mapbox Studio"
  />
}}

4. Click the **Style** tab at the top of the panel.
4. 单击面板顶部的 **Style** 选项卡。
5. The meteorite strike tileset is now a heatmap layer in your style. Click on the layer name and change it to "meteorite strikes".
5. 现在热力图图层是您创建的风格。单击图层名称并将其更改为“meteorite strikes”。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialLayerName"
    alt="Screenshot showing how to change a layer name in Mapbox Studio"
  />
}}

## Configure layer properties
## 配置图层属性
<!-- copyeditor ignore represents -->
A heatmap layer in Mapbox Studio has several different layer property options. Understanding what these properties mean is key to creating a heatmap that accurately represents your data and strikes the right balance between showing too much detail and being a large colorful blob!
Mapbox Studio中的热力图图层具有多个不同的图层属性选项。创建准确展示数据的热图的关键是了解这些图层的作用，并且有助于平衡显示太多细节和生成大型彩色斑点。

- **Color**: Defines the heatmap’s color gradient, from a minimum value to a maximum value. You can adjust the density and color of each stop individually, as well as add and delete stops.
- **颜色**: 定义热力图的颜色渐变，从最小值到最大值。您可以单独调整每个停靠点的密度和颜色，以及添加和删除停靠点。
- **Opacity**: Controls the global opacity of the heatmap layer.
- **不透明度**: 控制热力图图层的全局不透明度。
- **Radius**: Sets the radius for each point in pixels. As the radius number increases, the heatmap will get smoother and less detailed.
- **半径**: 设置每个点的半径（以像素为单位）。随着半径数的增加，热力图结果将变得更平滑，忽略一些细节。
- **Weight**: Measures how much each individual point contributes to the appearance of your heatmap. Heatmap layers have a weight of one by default, which means that all points are weighted equally. Increasing the weight property to five has the same effect as placing five points in the same location. You can use the _Style across data range_ and _Style with data conditions_ options to set the weight of your points based on specified properties.
- **权重**: 测量每个点对热图外观的影响程度。默认情况下，热力图图层的权重为1，这意味着所有点的权重均等。将权重属性增加到五与将五个点放在同一位置具有相同的效果。您可以使用样式跨数据范围和样式与数据条件选项来根据指定的属性设置点的权重。
- **Intensity**: A multiplier on top of the weight property. Intensity is primarily used as a convenient way to adjust the appearance of the heatmap based on zoom level.
- **强度**: 权重属性之上的乘数。强度主要用来根据缩放级别调整热力图外观的便捷方式。

### Radius
### 半径

In a heatmap, the radius of the points should increase as the zoom level increases to preserve the smoothness of the heatmap as the points become more spread out. For this tutorial, you will change the default (a radius of `30 px` at all zoom levels) so that the radius increases as the map is zoomed in.
在热力图中，点的半径应随着缩放级别的增加而增加，以便在点变得更加分散时保持热力图的平滑度。对于本教程，您将更改默认值（所有缩放级别的半径为30像素），以便在放大地图时半径增加。

1. Select the **Radius** tab in the heatmap layer.
1. 选择热图图层中的 **Radius** 选项卡。
2. Choose **Style across zoom range**.
2. 选择 **Style across zoom range**。
3. For the first stop, change the radius at zoom level `0` (zoomed out to the maximum extent) to `5 px`.
3. 对于第一个停止点，将缩放级别0（缩小到最大范围）的半径更改为5像素。
4. Click **Done**.
4. 单击  **Done**。
5. The next stop is already set at zoom level `22` (zoomed in to the maximum extent) with a radius of `30 px`. Leave this stop with these settings.
5. 下一个停止位置已设置为缩放级别22（放大到最大范围），半径为30像素。使用这些设置保留此停止。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialRadius"
    alt="Screenshot showing how to adjust heatmap data point radius in Mapbox Studio"
  />
}}

### Weight
### 权重

In the tileset you uploaded, the `mass(g)` property ranges from 1-60000000 grams. That's a wide range of meteorite sizes! Give more weight in your heatmap to the larger meteorites by setting stops that increase the weight of a point as the mass increases.
在您上传的tileset中，mass（g）属性的范围为1-60000000克。这是各种各样的陨石尺寸。通过设置随着质量增加而增加点权重的挡块，使热力图中的权重更大。

1. Select the **Weight** tab.
1. 选择 **Weight** 选项卡。
2. Click on **Style across data range**.
2. 单击 **Style across data range**。
3. You will be prompted to choose a numeric data field on which to base the property's styling. Choose `mass(g)`.<br><br>
3. 系统将提示您选择一个数字数据字段作为该属性的样式。选择质量（g）。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialDataRange"
    alt="Screenshot showing how to choose a data field in the style across data range option"
  />
}}

4. The first stop is set to a mass of `0` and a weight of `1`. Leave this stop at these settings.
4. 第一个停止点设置为质量0，权重为1。这一步的设置到此为止。
5. The next stop is already set to the maximum mass in the dataset. Change the weight to `25`.
5. 下一个停止点设置为数据中的最大质量。将权重更改为25。
6. Click `Done`.
6. 单击 `Done`。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialWeight"
    alt="Screenshot showing how to adjust heatmap weight in Mapbox Studio"
  />
}}

### Intensity
### 强度

The intensity of the heatmap layer can be increased as the map zooms in, to preserve a similar appearance throughout the zoom range.
当地图放大时，热图图层的强度可以增加，以在整个地图范围内保持类似的外观。

1. Select the layer's **Intensity** tab.
1. 选择图层的 **Intensity** 选项卡。
2. Click on **Style across zoom range**.
2. 单击 **Style across zoom range**。
3. The first stop is set to a zoom level of `0` and an intensity of `1`. Leave this as it is.
3. 第一个停止点设置为缩放级别0和强度为1。这一步的设置到此为止。
4. Change the second stop to zoom level `22` and an intensity of `10`.
4. 将第二个停止更改为缩放级别22，强度为10。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialIntensity"
    alt="Screenshot showing how to adjust heatmap intensity in Mapbox Studio"
  />
}}

These images show the impact of intensity on your style's appearance. The image on the right shows intensity that uses the default of `1`, while the image on the left shows intensity that increases with zoom level.
这些图像显示了强度对你的风格外观的影响。右侧的图像显示使用默认值1的强度，而左侧的图像显示随着缩放级别增加的强度。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialIntensityComparison"
    alt="Screenshot showing the difference between default and custom heatmap intensity in Mapbox Studio"
  />
}}

### Color
### 颜色
Heatmap layers in Mapbox Studio come with a vibrant set of preset colors with which to represent the density of the data points. For this tutorial, though, we will change the color scheme. We've taken these colors from a color recommendation site that's specifically for cartography, [Color Brewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3).
Mapbox Studio 中的热力图图层带有一组充满活力的预设颜色，用于表示数据点的密度。但是，对于本教程，我们将更改颜色方案。我们从色彩推荐网站上采用了这些颜色，专门用于制图，Color Brewer。

1. Select the **Color** tab in the heatmap layer.
1. 选择热图图层中的 **Color** 选项卡。
2. The first stop has a density of `0` and an opacity of `0`. Without these settings, the meteorite layer would be opaque, obscuring the rest of the map. Leave this stop at these settings.
2. 第一个停止点设置的密度为0，不透明度为0.如果没有这些设置，陨石层将是不透明的，会遮挡地图的其余部分。这一步的设置到此为止。
3. Click on the second stop, which has a density of `0.1`. Change the color to {{<ColorSwatch color="#ffffb2" />}}.
3. 单击第二个停止点，其密度为0.1。将颜色更改为#ffffb2。
4. Click **Done**.
4. 单击 **Done**。
5. Adjust the remaining density stops with the following colors:
5. 使用以下颜色调整剩余密度停止点：
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

### Opacity
### 不透明度
Your heatmap is looking good, but it's difficult to read location labels since the meteorite strike points obscure them. Adjust the heatmap layer's opacity so that you can read the labels:
热力图看起来不错，但由于陨石撞击点互相重合，因此很难读取位置标签。调整热力图图层的不透明度，以便您可以读取标签：

1. Select the **Opacity** layer.
1. 选择 **Opacity** 图层。
2. Change the opacity to `0.7`, either by using the slider bar or by typing the number in directly.
2. 通过使用滑块或直接键入数字，将不透明度更改为0.7。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialOpacity"
    alt="Screenshot showing how to change heatmap layer opacity in Mapbox Studio"
  />
}}

## Publish
## 发布

When you finish editing your map style, publish your changes by clicking **Publish** in the upper right side of the screen. When you click the publish button, a window will display the difference between the previous and current version of this style. If you're happy with the changes, click **Publish**. Your style will now be available to share from a variety of tools and applications.
完成地图样式的编辑后，通过单击屏幕右上角的 **Publish** 来发布更改。单击 **Publish** 按钮时，窗口将显示此样式的上一个版本和当前版本之间的差异。如果您对更改感到满意，请单击 **Publish** 。您创建的风格现在可以从各种工具和应用程序中共享。

{{
  <AppropriateImage
    imageId="studioHeatmapTutorialPublish"
    alt="Screenshot showing how to change heatmap stop colors in Mapbox Studio"
  />
}}

If you go back to your Styles page, you will see your new style at the top of the list.
如果您返回 Styles 页面，您将在列表顶部看到新样式。

## Finished product
## 完成

You have created a style that displays meteorite strike data from around the world, in a way that also allows you to show the relative mass of each meteorite.
您创建了一种显示来自世界各地的陨石撞击数据的风格样式，同时还允许您显示每个陨石的相对质量。

{{
  <DemoIframe src="https://api.mapbox.com/styles/v1/examples/cjikt35x83t1z2rnxpdmjs7y7.html?access_token=MapboxAccessToken#2/11.285/-65.322/0" />
}}


## Next steps
## 下一步

Mapbox Studio gives you a variety of ways to use the new map style you created. You can use this map directly on a website or in a web or mobile application. Take a look at the [Publish style section](https://docs.mapbox.com/studio-manual/overview/publish-your-style/) of the Mapbox Studio Manual to learn about the different ways you can use your style.
Mapbox Studio 为您提供了使用您创建的新地图样式的各种方法。您可以直接在网站或 Web 或移动应用程序中使用此地图。请查看Mapbox Studio手册的 [Publish style section](https://docs.mapbox.com/studio-manual/overview/publish-your-style/) 部分，了解使用风格样式的不同方法。
