---
title: Get started with Mapbox GL JS expressions
description: Learn how to write expressions in Mapbox GL JS.
thumbnail: glJsExpressions
level: 2
prereq: Familiarity with front-end development concepts.
topics:
- web apps
- map design
language:
- JavaScript
prependJs:
  - "import * as constants from '../../constants';"
  - "import Note from '@mapbox/dr-ui/note';"
  - "import BookImage from '@mapbox/dr-ui/book-image';"
  - "import Icon from '@mapbox/mr-ui/icon';"
  - "import UserAccessToken from '../../components/user-access-token';"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
  - "import Button from '@mapbox/mr-ui/button';"
contentType: tutorial
---

在这个指南中您将会学到如何在 Mapbox GL JS 中通过编写表达式实现基于数据属性和缩放级别来自定义数据样式。

{{
  <DemoIframe src="/help/demos/intro-to-expressions/step-four.html" />
}}

## 准备工作

- **一个 Mapbox 账号和 access token** 。在 [mapbox.com/signup](https://www.mapbox.com/signup/) 注册一个账号，您可以在 [Account page](https://www.mapbox.com/account/) 中找到您的 [access tokens](/help/how-mapbox-works/access-tokens/) 。
- **Mapbox GL JS** 。您需要使用我们的 JavaScript 软件库—— Mapbox GL JS 。
- **A style URL** 。您在初始化地图时需要一个 [style URL](/help/glossary/style-url/) 。您可以在 _Add a designer style to your account_ 中找到有关 style URL 的步骤指导。
- **数据**。在本教程中，您将会用到 [HPC Landmarks from Open Minneapolis](http://opendata.minneapolismn.gov/datasets/hpc-landmarks/data) 中的 CSV 格式文件。

{{
<Button href="/help/demos/intro-to-expressions/HPC_landmarks.csv" passthroughProps={{ download: "HPC_landmarks" }} >
    <Icon name='arrow-down' inline={true} /> Download the data
</Button>
}}

## 什么是表达式？

在 Mapbox 样式规范中，可以将任何布局属性，绘图属性或筛选器的值以表达式的形式显示。表达式定义一个或多个要素属性值和/或当前缩放级别如何使用逻辑，数学，字符串或颜色操作来进行组合，以生成适当的样式属性值或筛选结果。

**属性表达式**是使用对要素属性数据的引用来定义的任何表达式。属性表达式允许要素的外观随其属性而变化。可用于在视觉上区分同一层中的不同要素类型或创建数据可视化。

{{
  <Note title='属性表达式 vs. 属性函数' imageComponent={<BookImage />}>
    <p>如果您曾经使用过 Mapbox GL JS ，您会对属性 <em>functions</em> 熟悉。属性 <em>expressions</em> 可以帮助您实现与 <em>functions</em> 类似的效果，具有更多的灵活性和功能。属性表达式在 <a href='https://github.com/mapbox/mapbox-gl-js/blob/master/CHANGELOG.md#0410-october-11-2017'>Mapbox GL JS v0.41.0</a> 中引入。虽然属性函数目前仍可使用，但它们最终将被弃用并被属性表达式替换。</strong></p>
  </Note>
}}

### 用途

有许多方法可以将属性表达式应用于您的应用程序，包括：

- **数据驱使的样式定义（Data-driven styling）**：根据一个或多个数据属性定义样式规则。
- **算术运算（Arithmetic）**：对源数据进行算术运算，例如执行转换单位的计算。
- **条件逻辑（Conditional logic）**：使用if-then逻辑，例如，根据要素中可用的属性甚至名称的长度，准确确定要为标签显示的文本。
- **字符串操作（String manipulation）**：无需编辑，重新准备和重新上传数据，使用大写，小写和标题大小写转换来控制标签文本。

### 句法

Mapbox GL JS 表达式使用类似 Lisp 的语法，使用JSON数组表示。表达式遵循以下格式：

```
[expression_name, argument_0, argument_1, ...]
```

`expression_name` 是**表达式运算符**，例如，您可以使用 [`'*'`](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-*) 乘以两个参数或 [`'case'`](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-case) 来创建条件逻辑。有关所有可用表达式的完整列表，请参阅 [Mapbox Style Specification](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions) 。

**参数**可以是 _literal_ (数字，字符串或布尔值)，也可以是自身表达式。参数的数量因表达式而异。

这是使用表达式计算算术表达式 (π * 3<sup>2</sup>) 的一个示例：

```
['*', ['pi'], ['^', 3, 2]]
```

此示例使用 [`'*'`](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-*) 表示将两个参数相乘。第一个参数是 [`'pi'`](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-pi) ，它是一个返回数学常数pi的表达式。第二个参数是 [`'^'`](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-^) 表达式，它有两个自己的参数。它将返回 3<sup>2</sup> ，结果将乘以π。

## 设置地图

现在您已经初步了解表达式的用法和语法，眼过千遍不如手过一遍，使用 Mapbox GL JS 初始化地图，并将自定义数据添加到圆形图层（circle layer）。

### 为您的帐户添加设计师样式

本指南中的地图使用了 Mapbox 的设计师风格之一。要使用设计师样式，您需要先将其添加到您的帐户。登录您的 Mapbox 帐户并访问设计师地图页面，为您的帐户添加新样式：

1. 访问 [mapbox.com/designer-maps](https://www.mapbox.com/designer-maps/) 。
2. 找到 _North Star_ 样式，然后单击 **Add this style** 。
3. 该样式将自动添加到您的 Mapbox 帐户。

将样式添加到您的帐户后，它将显示在 Mapbox Studio 的样式页面上。从这里，您可以找到样式的 [style URL](/help/glossary/style-url) 。您将使用样式URL将此样式添加到地图中：

1. 在 [Styles page](https://www.mapbox.com/studio/styles) 页面上找到样式。
2. 单击样式名称旁边的 **Menu {{<Icon name='menu' inline={true} />}}** 。
3. 找到 _Style URL_ 。您可以使用剪贴板图标 {{<Icon name='clipboard' inline={true} />}} 复制样式URL，您将在 _Initialize a map with data_ 选项中使用它。

### 上传数据

在本指南中，您将使用 [vector tileset](/help/glossary/tileset) 在应用程序中显示数据。您可以通过将之前下载的 CSV 上传到 Mapbox Studio 来创建vector tileset：

1. 访问 Mapbox Studio 中的 [Tilesets page](https://www.mapbox.com/studio/tilesets) 页面。
2. 点击 **New tileset** 。
3. 选择在本教程开头下载的 CSV 文件，然后单击 **Confirm** 。
4. 右下方会显示一个弹出窗口，显示上传的进度。
5. 上传显示 _Succeeded_ 后，切片数据集即可使用。通过单击弹出窗口中切片数据集的名称来打开数据集的信息页面。
6. 记下切片数据集信息页面右侧的 *tileset ID* 。在下一步中，您将通过使用此ID将此切片数据集添加到地图中

### 使用数据初始化地图

在文本编辑器中，创建一个新的 `index.html` 文件并使用以下代码初始化您的地图。您需要：

1. 确保将 `mapboxgl.accessToken` 设置为等于您的一个 [access tokens](/help/glossary/access-token/) 。
2. 将 `your-style-url-here` 替换为您的 [style URL](/help/glossary/style-url/) 。
3. 将 `your-tileset-id-here` 替换为您创建的切片数据集的 [tileset ID](/help/glossary/tileset-id) 。
4. 将您的 `your-source-layer-here` 替换为切片数据集中 [source layer](/help/glossary/source-layer/) 的名称。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8' />
    <title></title>
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />
    <style>
      body {
        margin: 0;
        padding: 0;
      }

      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
    </style>
  </head>
  <body>

  <div id='map'></div>
  <script>
  mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
  var map = new mapboxgl.Map({
    container: 'map', // container id
    style: 'your-style-url-here', // stylesheet location
    center: [-93.261, 44.971], // starting position [lng, lat]
    zoom: 10 // starting zoom
  });

  map.on('load', function() {
    map.addLayer({
      id: 'historical-places',
      type: 'circle',
      source: {
        type: 'vector',
        url: 'mapbox://your-tileset-id-here'
      },
      'source-layer': 'your-source-layer-here',
    });
  });

  </script>

  </body>
</html>
```

在浏览器中运行您的应用程序，您将看到以明尼阿波利斯为中心的浅色地图，整个城市地图上方散布着黑色圆点。

{{
  <DemoIframe src="/help/demos/intro-to-expressions/step-one.html" />
}}

## 编写属性表达式

接下来，您将编写一个表达式使每个圆的半径根据每个历史地标的存在时间设置。数据由明尼阿波利斯市的开放数据门户网站提供，未提供历史地标的年龄，但提供了建造年份。您可以使用算术根据当前年份计算历史地标的存在时间，并根据存在时间设置 `circle-radius` 绘制属性的样式。

### 计算每个地标的存在时间

首先计算地标的存在时间。可以使用下列表达式计算：

```
['-', 2017, ['number', ['get', 'Constructi'], 2017]]
```

使用带有两个参数的 `'-'` 表达式。第一个参数是一个数字（当前年份，2017年）。第二个参数是另一个带有两个参数的表达式（一个 `'number'` 表达式）。此表达式将第一个参数转换为数字。如果第一个参数没有值，那么将使用第二个参数，即当前年份2017。

`'number'` 表达式的第一个参数是另一个表达式 &mdash；这次是一个 `'get'` 表达式，它返回其唯一参数 `'Constructi'` 的对象属性值。 （`'Constructi'` 是原始CSV文件中的 `'Construction_Date'` ，但在上传过程中缩短了字段的名称。）使用 `'get'` 返回的值变为 `'number'` 中的一个参数。

最终，表达式输出地标的存在时间，结果呈现为`2017年 - 建造年份`。

### 指定圆半径

在代码中现有的 addLayer 函数中使用此表达式：

```js
map.on('load', function() {
  map.addLayer({
    id: 'historical-places',
    type: 'circle',
    source: {
      type: 'vector',
      url: 'mapbox://your-tileset-id-here'
    },
    'source-layer': 'your-source-layer-here',
    paint: {
      'circle-radius': ['-', 2017, ['number', ['get', 'Constructi'], 2017]],
      'circle-opacity': 0.8,
      'circle-color': 'rgb(171, 72, 33)'
    }
  });
});
```

<!-- copyeditor ignore built-->
刷新浏览器，您会发现圆半径会发生显著变化。使用当前表达式，表示在1950年建立的地标的圆的半径将是67像素。下一步，您将在下文的说明中调整以获得更合适的半径。

{{
  <DemoIframe src="/help/demos/intro-to-expressions/step-two.html" />
}}

### 调整圆半径

将表达式换行到另一个表达式中，以使圆圈的大小看起来更清晰合适。在这个例子中，您可以将地标的存在时间数字除以`10`。

```js
map.on('load', function() {
  map.addLayer({
    id: 'historical-places',
    type: 'circle',
    source: {
      type: 'vector',
      url: 'mapbox://your-tileset-id-here'
    },
    'source-layer': 'your-source-layer-here',
    paint: {
      'circle-radius': [
        '/',
        ['-', 2017, ['number', ['get', 'Constructi'], 2017]],
        10
      ],
      'circle-opacity': 0.8,
      'circle-color': 'rgb(171, 72, 33)'
    }
  });
});
```

刷新浏览器，您将看到更新后的地图。

{{
  <DemoIframe src="/help/demos/intro-to-expressions/step-three.html" />
}}

## 添加缩放表达式

在开始缩放级别`10.5`时，圆圈仍然重叠面积较多，但它们在更高的缩放级别上看起来很好。您可以使用缩放表达式来解决此问题。 **zoom expression** 是使用`['zoom']`定义的任何表达式。此类表达式允许图层的外观随地图的缩放级别变化而变化。缩放表达式可用于创建深度和控制数据密度的错觉。

使用 [`'interpolate'`](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions-interpolate) 表达式。 `'interpolate'` 表达式通过在一对输入和输出值之间进行插值（“stops”）来产生连续，平滑的结果。在这种情况下，使用 `['linear']` 在略微小于和略大于输入的一对参数之间线性插值。然后，指定半径在缩放级别`10`时显示 `age of the landmark / 30` ，在缩放级别`13`时显示 `age of the landmark / 10` 。

```js
map.on('load', function() {
  map.addLayer({
    id: 'historical-places',
    type: 'circle',
    source: {
      type: 'vector',
      url: 'mapbox://your-tileset-id-here'
    },
    'source-layer': 'your-source-layer-here',
    paint: {
      'circle-radius': [
        'interpolate', ['linear'], ['zoom'],
        10, ['/', ['-', 2017, ['number', ['get', 'Constructi'], 2017]], 30],
        13, ['/', ['-', 2017, ['number', ['get', 'Constructi'], 2017]], 10],
      ],
      'circle-opacity': 0.8,
      'circle-color': 'rgb(171, 72, 33)'
    }
  });
});
```

{{
  <Note imageComponent={<BookImage />}>
    <p>如果您之前使用过 <a href='https://www.mapbox.com/mapbox-gl-js/style-spec/#types-function'>property <em>functions</em></a> ，请注意 <code>interpolate</code> 表达式允许您实现与停止函数相同的效果。</p>
  </Note>
}}

刷新浏览器和放大地图，您将看到处理后的地图结果。

{{<img src='/help/img/gl-js/expressions-step-four.gif' alt='animated screenshot zooming in and out and panning around a map with custom data styled using expressions' className='mx-auto block' />}}

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8' />
    <title>Minneapolis Landmarks</title>
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />
    <style>
      body {
        margin: 0;
        padding: 0;
      }

      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
    </style>
  </head>
  <body>

  <div id='map'></div>
  <script>
    mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
    var map = new mapboxgl.Map({
      container: 'map', // container id
      style: 'your-style-url-here', // stylesheet location
      center: [-93.261, 44.971], // starting position [lng, lat]
      zoom: 10.5 // starting zoom
    });

    map.on('load', function() {
      map.addLayer({
        id: 'historical-places',
        type: 'circle',
        source: {
          type: 'vector',
          url: 'mapbox://your-tileset-id-here'
        },
        'source-layer': 'your-source-layer-here',
        paint: {
          'circle-radius': [
            'interpolate', ['linear'], ['zoom'],
            10, ['/', ['-', 2017, ['number', ['get', 'Constructi'], 2017]], 30],
            13, ['/', ['-', 2017, ['number', ['get', 'Constructi'], 2017]], 10],
          ],
          'circle-opacity': 0.8,
          'circle-color': 'rgb(171, 72, 33)'
        }
      });
    });
  </script>
  </body>
</html>
```

## 成品

您已经使用 Mapbox GL JS 中的表达式设置了自定义数据样式！

{{
  <DemoIframe src="/help/demos/intro-to-expressions/step-four.html" />
}}

## 下一步

在 Mapbox GL JS 中还有许多其他方法可以使用表达式。可以阅读以下内容获得更多信息：

- 在 [Mapbox Style Specification](https://www.mapbox.com/mapbox-gl-js/style-spec/#expressions) 中阅读有关表达式的内容。
- 尝试使用其他表达式的 [Mapbox GL JS examples](https://www.mapbox.com/mapbox-gl-js/example/simple-map/) ：
  - 创建和设置群集样式 [Create and style clusters](https://www.mapbox.com/mapbox-gl-js/example/cluster/)
  - 创建悬停效果 [Create a hover effect](https://www.mapbox.com/mapbox-gl-js/example/hover-styles/)
  - 创建时间滑块 [Create a time slider](https://www.mapbox.com/mapbox-gl-js/example/timeline-animation/)
  - 过滤地图视图中的要素 [Filter features within map view](https://www.mapbox.com/mapbox-gl-js/example/filter-features-within-map-view/)
  - 通过缩放级别更新分级统计显示图层 [Update a choropleth layer by zoom level](https://www.mapbox.com/mapbox-gl-js/example/updating-choropleth/)
