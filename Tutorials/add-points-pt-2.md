---
title: "Add points to a web map, part 2: create a style"
description: Add a tileset to a template style and upload custom icons in Mapbox Studio.
thumbnail: addPointsPt2
level: 1
topics:
- uploads
- map design
language:
- No code
prependJs:
  - "import * as constants from '../../constants';"
  - "import AppropriateImage from '../../components/appropriate-image';"
  - "import Icon from '@mapbox/mr-ui/icon';"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
  - "import Button from '@mapbox/mr-ui/button';"
  - "import { Video } from '../../components/video';"
  - "import Publish from '../../video/add-points-pt-2-publish.mp4';"
contentType: tutorial
---

这是在[系列教程](https://docs.mapbox.com/studio-manual/help/#add-points-to-a-map)中的第二部分，在这一部分中将会教你如何使用Mapbox工作空间数据集编辑器、Mapbox工作空间样式编辑器和Mapbox GL JS.在网络地图中添加点。
*第二部分*Mapbox工作空间样式编辑器。在本教程中，你将会学习如何：

- 利用Mapbox默认样式创建一个新的样式
- 添加一个地形数据集作为图层
- 设置新图层样式
- 发布样式

{{
<DemoIframe src="https://api.mapbox.com/styles/v1/examples/cjgiiz9ck002j2ss5zur1vjji.html?access_token=MapboxAccessToken#10.7/41.893748/-87.661557/0" />
}}

## 准备

以下是在使用教程中将会用到的一些资源：

- **地形数据集** 你将会需要一个包含10个芝加哥公园的地形数据。你可以学习如何去创建这个地形数据集，在 [添加点到Web地图，第1部分：创建数据集](/help/tutorials/add-points-pt-1)。
- **自定义图标。** 本教程使用自定义图标显示公园的位置。 您需要下载SVG图标才能在您的风格中使用。

{{
<Button href="/help/data/marker-editor.svg" passthroughProps={{ download: "marker-editor" }} >
    <Icon name='arrow-down' inline={true} /> Download SVG icon
</Button>
}}

## 将数据添加到一个样式中

Web地图由[地图图件](/help/how-mapbox-works/web-apps/)组成。要将数据添加到Web地图，Mapbox会将其切割为切片，以便数据可以以各种缩放级别显示。 Mapbox将地图切割成的图块集合称为图块集。 [在向Web地图添加点，第1部分：创建数据集](/help/tutorials/add-points-pt-1)时，您将数据集转换为地形数据集以将其添加到新的地图样式。

### 创建一个新样式

在[完成向网络地图添加点,第一部分：创建一个数据集](/help/tutorials/add-points-pt-1)后，利用基础模板创建一个新的样式。在地图框工作室的[样式](https://studio.mapbox.com/styles)页面上，单击**新建样式**按钮。找到 _基本模板_ 样式，然后单击**自定义基本模板**。

样式编辑器会自动打开。使用屏幕左上角的标题字段将新样式重命名为Chicago Parks

{{
  <AppropriateImage
    imageId="addPointsPt2RenameStyle"
    alt="Mapbox Studio style editor showing how to rename a style"
  />
}}

### 创建一个新的图层

你将会添加你的地形数据作为一个新的图层：

1. 当样式编辑器打开时，点击在左上的 **+添加图层**。
2. 在数据资源列表中，找到你的 **chicago-parks** 地形数据集，点击数据集的名字，然后点击显示的源图层，将其添加为图层的源。

{{
  <AppropriateImage
    imageId="addPointsPt2AddLayer"
    alt="screenshot illustrating how to add a new layer in Mapbox Studio"
  />
}}

3. Mapbox 工作空间可识别您上传的数据集中在不同的位置，因此会显示消息 _“此地图视图中无法使用此图块集”_ 。点击**Go to data**，地图视图将重新聚焦芝加哥。
4. 点击**类型**选项，然后选择 _符号_ 图层选项，以便创建带有[标记](/help/glossary/marker/)的图层。
5. 点击返回**样式**选项卡。 

{{
  <AppropriateImage
    imageId="addPointsPt2CreateLayer"
    alt="screenshot illustrating how to create a new layer in Mapbox Studio"
  />
}}

## 图层样式

在Mapbox工作空间样式编辑器中，您可以指定每个图层的样式属性。这包括Mapbox默认样式中的图层以及您使用自定义数据添加的任何图层。对于此示例，您将设置*符号*图层的样式。 您可以使用文本和图标设置符号图层样式。

### 将样式作为一个符号图层

1. 点击在样式编辑器左侧的图层列表中创建的**chicago-parks** 图层。
2. 样式面板打开后，如果您尚未存在，请单击**样式**选项卡。
3. 选择**图标**选项卡，然后点击**spritesheet中的管理图标**。
4. 这个可以打开最顶部工具栏的**图片**设置。

{{
  <AppropriateImage
    imageId="addPointsPt2UploadSvg"
    alt="screenshot demonstrating the upload SVG menu in Mapbox studio"
  />
}}

5. 点击**下载SVG图片**按钮，从文件中选择您在本教程开头下载的标记。
6. 图标下载完成后，在列表中选择它。

{{
  <AppropriateImage
    imageId="addPointsPt2SelectIcon"
    alt="screenshot demonstrating the upload SVG menu in Mapbox studio"
  />
}}

7. 如果要显示所有标记，即使它们重叠，也请单击**放置**选项卡。向下滚动到**允许图标重叠**并将其设置为`True`。

您现在应该会在所有点上看到标志。

{{
  <AppropriateImage
    imageId="addPointsPt2IconsLoaded"
    alt="screenshot showing a style with a custom icon loaded in Mapbox Studio"
  />
}}

## 发布样式

现在您已经完成了地图的样式设置，您需要发布您的样式以使这些更改在Web上生效。

1. 单击编辑器右上角的**发布**按钮。
2. 弹出一个窗口，要求您查看更改。
3. 点击**发布**。

{{
  <Video
    filename={Publish}
    title="Comparing styles in the Publish modal"
  />
}}

## 共享

[共享模块](https://docs.mapbox.com/studio-manual/overview/publish-your-style/)包括在Web应用程序，移动应用程序或其他第三方工具中发布样式所需的资源。

### 共享

单击样式编辑器右上角的**共享**。共享模块包含一个共享URL，允许您与其他人共享样式预览。该模块还包含样式URL和访问令牌，本教程的[第三部分](/help/tutorials/add-points-pt-3/)中都需要这两个样式。

{{
  <AppropriateImage
    imageId="addPointsPt2ShareStyle"
    alt="Screenshot showing the share section for a style in Mapbox Studio"
  />
}}

## 接下来

前往[将点添加到Web地图，第3部分：添加交互性](/help/tutorials/add-points-pt-3/)以使用Mapbox GL JS向您的地图添加有关每个公园的信息的弹出窗口。
