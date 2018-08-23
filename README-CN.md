# Street Map Plugin for UE4

此插件允许您将OpenStreetMap XML数据作为新的StreetMap资产类型导入到虚幻引擎4项目中。您可以使用示例街道地图组件来渲染街道和建筑物。
![UE4OSMBrooklyn](Docs/UE4OSMBrooklyn.png)

![UE4OSMRaleigh](Docs/UE4OSMRaleigh.png)

Have fun!!  --[Mike](http://twitter.com/mike_fricker)

*玩的开心！！- 迈克

（注意：这个插件只是一个有趣的假期项目，并没有得到Epic的正式支持。）*


## 快速开始
它很容易启动和运行：

从此页面下载StreetMap插件源（单击克隆或下载 - > 下载ZIP）。

将文件解压缩到项目的Plugins文件夹下的新StreetMap子文件夹中。它应该看起来像“/ MyProject / Plugins / StreetMap /”

重建您的C ++项目。新的插件也将被编译！

加载编辑器。您现在可以将OpenStreetMap XML文件（.osm）拖放到内容浏览器中以导入地图数据！

将导入的街道地图数据资产拖放到视口中，将自动生成街道地图演员。您现在应该在3D视口中看到您的街道和建筑物。

![UE4OSMManhattan](Docs/UE4OSMActor.png)


如果重建成功但您没有看到新功能，请再次单击“设置”工具栏按钮检查“ 街道地图”插件是否已启用，然后单击“ 插件”。找到街道地图插件并确保选中已启用。

如果您不熟悉UE4中的插件，可以在此处找到大量信息。

## 获取OpenStreetMap数据

法律： OpenStreetMap数据根据ODC开放数据库许可（ODbL）获得许可。如果您在项目中使用此数据，请确保您了解并遵守该许可的条款，例如查找法律常见问题解答。

![UE4OSMExport](Docs/UE4OSMExport.png)

以下是如何获取您感兴趣的位置的数据：

对于较大的区域（超过邻居或小镇），您应该使用Mapzen Extracts。

转到OpenStreetMap.org并使用搜索功能导航到您在地球上最喜欢的位置。

单击页面顶部导航栏上的“ 导出”按钮进入“ 导出模式”。

滚动和缩放，以便您要导出的区域填充浏览器窗口。从相当小的东西开始，以便快速完成导出和下载。尝试放大小镇或城市街区。

准备好后，单击左侧的“ 导出 ”。OpenStreetMap将生成一个XML文件并很快启动下载。

如果要微调保存的矩形，可以在OpenStreetMap窗口中单击“手动选择其他区域”，然后在要导出的地图上调整矩形。

请记住，许多位置可能只有有限的建筑几何信息。特别是，在许多城市，建筑物的高度可能会丢失或不正确。

如果在单击“ 导出”后收到错误消息，则OpenStreetMap可能太忙而无法容纳请求。尝试单击Overpass API或检查其他来源之一。确保下载的文件具有扩展名“.osm”，因为这是插件所期望的。您可以根据需要重命名下载的文件。

当然，还有很多其他地方你可以在网上找到原始的OpenStreetMap XML数据，但请记住，插件仅使用直接从OpenStreetMap导出的文件进行了测试。

## 编辑OpenStreetMap
注意： OSM涵盖现实世界，仅包含基于事实的知识。如果您想构建一个虚构的地图，可以使用JOSM离线编辑器创建一个本地XML文件，您不会将其上传（！）到项目中。

您可以轻松回馈OSM，例如改善您的家乡。只需在www.openstreetmap.org注册，然后单击编辑选项卡。在线iD编辑器允许您跟踪航拍图像并轻松添加POI。要了解更多详情，请查看此处：

* http://learnosm.org
* https://wiki.openstreetmap.org/wiki/Video_tutorials

请注意，项目社区（居民！）是必不可少的部分。因此，明智的做法是与靠近你的地图集取得联系，以获得有关本地标记或不成文规则的更多提示。快乐的映射！

## 插件详细信息

### 街道地图资产

当您导入OSM文件，插件将创建一个新的街道地图的资产来表示UE4的地图数据。您可以将这些分配给街道地图组件，或直接与C ++代码中的地图数据交互。

使用完整的连接数据导入道路！这意味着您可以非常轻松地设计自己的导航算法。

OpenStreetMap位置数据存储在地理坐标（纬度和经度）中，但UE4本身不支持该坐标系。也就是说，我们目前无法轻易处理UE4中的球形世界。因此，在导入过程中，我们将所有地图坐标投影到平面2D平面。

OSM数据以双精度导入，但在保存UE4街道地图资产之前，我们会将所有内容截断为单精度浮点数。如果您计划在运行时使用大量的地图数据集，则需要对其进行修改。

### 街道地图组件

包括街道地图组件的示例实现，其从加载的街道和建筑物数据生成可渲染网格。这是一个非常简单的组件，您可以将其用作起点。

示例实现创建自定义基元组件网格而不是传统的静态网格。这样做的原因是为了允许城市街道和建筑物的更灵活的渲染行为，甚至动态方面。

所有网格数据都是在加载时根据地图资产中的制图数据生成的，包括彩色道路条和带有三角形屋顶多边形的简单建筑物网格。在道路上不执行样条插值。

生成的街道地图网格具有顶点颜色和法线，您可以为其指定自定义材质。如果要使用内置颜色，请确保材质将“顶点颜色”与“基色”相乘。网格设置为在单个绘制调用中非常有效地呈现。道路表示为简单的四边形条带（没有镶嵌）。尚不支持纹理坐标。

有各种“可调整”变量来控制可渲染网格的生成方式。您可以在UStreetMapComponent :: GenerateMesh（）函数体的顶部找到它们。

*（街道地图组件也可以作为如何在UE4中编写自己的原始组件的简单示例。）*


### OSM文件

在导入OpenStreetMap XML文件时，我们将所有有趣的数据存储在内存中的FOSMFile数据结构中。这包含与XML文件中的原始表示非常接近的数据。坐标以双精度浮点存储为地理位置。

在将所有内容加载到FOSMFile之后，我们将数据消化并将其转换为可以序列化到磁盘并在运行时高效加载的格式（UStreetMap类）。

根据您的使用情况，您可能需要大量自定义UStreetMap类来存储更接近地图原始表示的数据。例如，如果您想要执行大规模GPS导航，则需要在运行时提供更高精度的数据。

### 已知的问题
有各种松散的目的。

* 导入大于2GB的文件将崩溃。这是当前的UE4限制。

* 生成的OSM XML文件的某些变体无法正确加载。例如，不支持围绕值的单引号分隔符。

* 街道地图API应该易于使用C ++，但蓝图支持并不是这个插件的重点。许多方法都被内联以获得高性能。但是，如果有需求，可以添加蓝图脚本钩子。

* 如上所述，坐标被截断为单精度，这对于高级用例来说是不够的。同样，地理坐标不会在初始导入阶段之后保留。所有坐标都投影到一个平面上并转换为相对于地图边界矩形的中心。

* 运行时数据结构被设置为支持路径寻找（参见FStreetMapNode成员函数），但是还没有包括GPS算法的示例实现。

* 生成的网格数据目前非常简单，缺少碰撞信息，导航网格支持并且没有纹理坐标。这实际上只是作为一个例子而设计的。为了获得更多渲染灵活性和更快的性能，可以更改导入器以生成地图几何体的实际静态网格物体资源。

* 您可以在插件源代码中搜索@todo，以获取可能进行的其他微小改进。


### 兼容性

这个插件需要Visual Studio和C ++代码项目或GitHub的完整虚幻引擎4源代码。如果您不熟悉UE4中的编程，请参阅官方[编程指南](https://docs.unrealengine.com/en-us/Programming)！

街道地图插件应该适用于UE4支持的所有平台，但最新版本尚未在每个平台上进行测试。

我们将尝试使源代码保持最新，以便它可以与新版本的虚幻引擎一起发布。

## 支持
我不打算定期更新插件，但如果有任何关键修复，我肯定会尝试审查和集成它们。

对于错误，请提交问题，提交拉取请求或在Twitter上粉我。

最后，非常感谢给OpenStreetMap的基金会和梦幻般的社区谁贡献的地图数据和维护数据库。

I'm not planning to actively update the plugin on a regular basis, but if any critical fixes are contributed, I'll certainly try to review and integrate them.
 
For bugs, please [file an issue](https://github.com/ue4plugins/StreetMap/issues), submit a [pull request](https://github.com/ue4plugins/StreetMap/pulls?q=is%3Aopen+is%3Apr) or catch me [on Twitter](http://twitter.com/mike_fricker).

Finally, a **big thanks** to the [OpenStreetMap Foundation](http://wiki.osmfoundation.org/wiki/Main_Page) and the fantastic community who contribute map data and maintain the database.

