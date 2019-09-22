---
title: "Make a choropleth map, part 2: add interactivity"
description: Publish your style with Mapbox GL JS, create a legend, and add interactive elements.
thumbnail: choroplethStudioGlPt2
level: 2
topics:
- uploads
- map design
- web apps
language:
- JavaScript
prereq: Familiarity with front-end development concepts.
prependJs:
  - "import * as constants from '../../constants';"
  - "import Note from '@mapbox/dr-ui/note';"
  - "import BookImage from '@mapbox/dr-ui/book-image';"
  - "import Icon from '@mapbox/mr-ui/icon';"
  - "import UserAccessToken from '../../components/user-access-token';"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
contentType: tutorial
---
在 [part 1](/help/tutorials/choropleth-studio-gl-pt-1) 中，您在 Mapbox Studio 样式编辑器中设置了美国人口密度数据的样式，并发布了一个新样式。在第2部分中，您将使用 Mapbox GL JS 使这种样式在交互中变得生动起来。

{{
  <DemoIframe src="/help/demos/choropleth-studio-gl/demo-five.html" />
}}

## Mapbox Studio 和 Mapbox GL JS 如何协同工作
在上一份指南中，您使用 Mapbox Studio 样式编辑器设计地图并创建样式。但是当您点击发布时，这个软件生成了什么？

### 什么是样式？

样式是绘制网页地图最重要的部分：它包含了在网页上绘制哪些特征以及如何绘制这些特征的所有规则。 Mapbox Studio 和 Mapbox GL JS 都与您的样式直接交互： Mapbox Studio 样式编辑器是一个创建样式的可视化界面， Mapbox GL JS 用于将样式添加到网页中，并通过添加和更改图层和源以响应浏览器事件来直接与之交互。

在 [Mapbox Style Specification](https://www.mapbox.com/mapbox-gl-style-spec/) 中 [map style](/help/glossary/style) 是一个 JSON 对象，它包含浏览器正确绘制地图所需的所有内容。它的主要部分是：

- **`sources`**：链接将在地图上设置样式的所有数据。使用 Mapbox Studio 样式编辑器创建样式时，源是您的 Mapbox 帐户中的栅格和矢量切片集。
- **`sprite`**：指向样式中使用的所有图片和图标的链接。
- **`glyphs`**：指向样式中使用的所有字体的链接。
- **`layers`**：源中数据应如何显示在地图上的规则列表。

当您在第1部分中将人口密度数据添加到 Mapbox Studio 样式时，指向该数据的链接也添加到样式对象的源列表中（我们通常将其称为“样式”）。同样地，当您添加人口密度图层并为其提供每个数据类别的样式规则时，该图层也会添加到样式中的图层列表中。

## 开始

以下是您需要准备的内容：

- [**An access token**](/help/glossary/access-token) 。您可以在您的 [Account page](https://www.mapbox.com/account/) 上找到您的访问令牌。
- 您的样式的 [**style URL**](/help/glossary/style-url) 。在您的 [Styles](https://www.mapbox.com/studio) 页面，点击人口密度样式旁边的 **Menu {{<Icon name='menu' inline={true} />}}** 按钮，然后点击 {{<Icon name='clipboard' inline={true} />}} 剪贴板图标复制 **Style URL** 。
- [**Mapbox GL JS**](https://www.mapbox.com/mapbox-gl-js) 。用于构建网页地图的 Mapbox JavaScript API 。
- **A text editor** 。您毕竟要编写 HTML、CSS和JavaScript 。

##创建网页

打开您的文本编辑器并创建一个名为 `index.html` 的文件。通过在头部添加 Mapbox GL JS 及其关联的 CSS 文件来配置文档：

```html
<script src='https://api.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
<link href='https://api.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />
```

接下来，标记页面以创建地图容器、信息框和图例：

```html
<div id='map'></div>
<div class='map-overlay' id='features'><h2>US population density</h2><div id='pd'><p>Hover over a state!</p></div></div>
<div class='map-overlay' id='legend'></div>
```

您还需要应用一些 CSS 来可视化布局的外观。这对于 `map` div 尤其重要，除非您给它一个 `height` ，否则它不会显示在页面上：

```css
body {
  margin: 0;
  padding: 0;
}

h2,
h3 {
  margin: 10px;
  font-size: 1.2em;
}

h3 {
  font-size: 1em;
}

p {
  font-size: 0.85em;
  margin: 10px;
  text-align: left;
}

/**
* Create a position for the map
* on the page */
#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}

/**
* Set rules for how the map overlays
* (information box and legend) will be displayed
* on the page. */
.map-overlay {
  position: absolute;
  bottom: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.8);
  margin-right: 20px;
  font-family: Arial, sans-serif;
  overflow: auto;
  border-radius: 3px;
}

#features {
  top: 0;
  height: 100px;
  margin-top: 20px;
  width: 250px;
}

#legend {
  padding: 10px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  line-height: 18px;
  height: 150px;
  margin-bottom: 40px;
  width: 100px;
}

.legend-key {
  display: inline-block;
  border-radius: 20%;
  width: 10px;
  height: 10px;
  margin-right: 5px;
}
```
在下一步中，您将把地图添加到页面中，项目将开始成形。

## 初始化地图

现在您已经向页面添加了结构，就可以开始编写一些 JavaScript 了！首先需要做的是添加您的 access token 。没有这个，剩下的代码就不能生效。_注意：以下所有代码都应该在 `script` 标记之间。_：

```js
mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
```
现在您已经添加了页面的结构，您可以将一个 Map 对象添加到 `map` div 中。请确保将 `your-style-url` 替换为您在本指南第1部分中创建的样式的 [style URL] (/help/glossary/style-url) ，否则代码不会生效！

```js
var map = new mapboxgl.Map({
  container: 'map', // container id
  style: 'your-style-url' // replace this with your style URL
});
```

{{
  <DemoIframe src="/help/demos/choropleth-studio-gl/demo-two.html" />
}}

## 添加附加信息
在一些项目中，有些地方您需要暂停一下：您把地图放在一个页面上！但是对于这个地图，您将添加两个附加信息让地图更加实用：一个图例和一个信息窗口来显示光标悬停的任何州的人口密度。

### “加载”事件

{{
  <Note title='什么是回调？' imageComponent={<BookImage />}>
    <p>在页面上初始化地图不仅仅是在 <code>map</code>div 中创建一个容器。它还告知浏览器请求您在第1部分中创建的 Mapbox Studio 样式。根据 Mapbox 服务器对该请求的响应速度，这可能需要花费不确定的时间，而代码中要添加的所有内容都依赖于加载到地图中的样式。因此，确保在执行更多代码之前加载样式非常重要。</p><p>幸运的是，地图对象可以告诉浏览器当地图的状态改变时发生的某些事件。其中一个事件是 <code>load</code> ，当样式加载到地图上时会触发该事件。通过 <code>map.on</code> 方法，您可以将事件放在一个 <a href='https://github.com/maxogden/art-of-node#callbacks'>callback function</a> 中，在 <code>load</code> 事件发生时将调用该函数，确保在该事件发生之前不会执行剩下的代码</p>。
</Note>
}}

为了确保剩下的代码可以执行，它需要包含在地图加载完成后执行的 *callback function* 中。

```js
map.on('load', function() {
  // the rest of the code will go in here
});
```
### 创建图层间隔和颜色数组

创建包含州数据的图层样式时使用的间隔的列表允许我们在之后的步骤中向地图添加图例。

_谨记: 这段代码位于 `load` 回调函数的内部！_

```js
var layers = ['0-10', '10-20', '20-50', '50-100', '100-200', '200-500', '500-1000', '1000+'];
var colors = ['#FFEDA0', '#FED976', '#FEB24C', '#FD8D3C', '#FC4E2A', '#E31A1C', '#BD0026', '#800026'];
```
### 添加图例
以下代码将向地图添加图例。为此，它将遍历您上面定义的图层列表，并根据图层的名称及其颜色为每个图层添加一个图例元素。

```js
for (i = 0; i < layers.length; i++) {
  var layer = layers[i];
  var color = colors[i];
  var item = document.createElement('div');
  var key = document.createElement('span');
  key.className = 'legend-key';
  key.style.backgroundColor = color;

  var value = document.createElement('span');
  value.innerHTML = layer;
  item.appendChild(key);
  item.appendChild(value);
  legend.appendChild(item);
}
```

{{
  <DemoIframe src="/help/demos/choropleth-studio-gl/demo-three.html" />
}}

### 添加信息窗口
当光标悬停在某个州上时，信息窗口应显示该州的人口密度信息。如果光标没有悬停在某个州上，则信息窗口将显示“悬停在某个州上！”

要执行此操作，请为 `mousemove` 事件添加一个监听器，识别光标所在位置的州（如果有的话），然后更新信息窗口：

```js
map.on('mousemove', function(e) {
  var states = map.queryRenderedFeatures(e.point, {
    layers: ['statedata']
  });

  if (states.length > 0) {
    document.getElementById('pd').innerHTML = '<h3><strong>' + states[0].properties.name + '</strong></h3><p><strong><em>' + states[0].properties.density + '</strong> people per square mile</em></p>';
  } else {
    document.getElementById('pd').innerHTML = '<p>Hover over a state!</p>';
  }
});
```

{{
  <DemoIframe src="/help/demos/choropleth-studio-gl/demo-four.html" />
}}

## 最后润色

差不多完成了！最后几步：

### 光标

添加一行代码，为地图提供默认指针光标。

```js
map.getCanvas().style.cursor = 'default';
```
### 地图边界

通过设置加载地图的边界，确保加载地图时显示美国大陆：

```js
map.fitBounds([[-133.2421875, 16.972741], [-47.63671875, 52.696361]]);
```
## 任务完成

您已经创建了一个交互式等值线地图！

{{
  <DemoIframe src="/help/demos/choropleth-studio-gl/demo-five.html" />
}}

干得好！有关 Mapbox Studio 的更多操作，请参阅 [Mapbox Studio Manual](https://www.mapbox.com/studio-manual/) 。有关 Mapbox GL JS 及其工作方式的更多信息，请阅读 [How web apps work](/help/how-mapbox-works/web-apps/) 指南。

