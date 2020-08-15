## 一、简介

### 1、Geoserver

​		GeoServer是Gis服务器中的一种，主要用于管理与发布图层。
​		GeoServer是OpenGIS Web服务器规范的J2EE实现，所以在运行GeoServer服务的前提是先要有JAVA环境，配置JDK环境。
​		GeoSerever特性兼容 WMS 和 WFS 特性；支持 PostgreSQL、 Shapefile 、 ArcSDE 、 Oracle 、 [VPF](https://baike.baidu.com/item/VPF) 、 MySQL 、 MapInfo ；支持上百种投影；能够将网络地图输出为 jpeg 、 gif 、 png 、 SVG 、 KML 等格式；能够运行在任何基于 J2EE/Servlet 容器之上；嵌入MapBuilder 支持 AJAX 的地图客户端OpenLayers；除此之外还包括许多其他的特性。

### 2、PostgreSQL

​		PostgreSQL（对象-关系数据库管理系统）是由加州大学伯克利分校计算机系开发的Postgres软件包发展而来的。PostgreSQL几乎支持所有SQL构件（包括子查询，事物和用户定义类型和函数），并且可以获得非常广阔范围的开发语言绑定（包括C，C++，Java,Perl,tcl,和Python）。在空间数据管理方面, PostgreSQL定义了一系列的几何数据类型, 包括点(point),线(line),线段(lseg), 方形(box), 闭合和开放路径(path),多边形(polygon), 圆(circle)。但是PostgreSQL提供的几何类型并不支持OpenGIS的SFS规范, 缺乏复杂几何类型, 没有提供空间分析和投影变换模块, 很难达到GIS的应用要求。

### 3、PostGIS

​		PostGIS（[http://www.postgis.org/](https://link.zhihu.com/?target=http%3A//www.postgis.org/)）是一个功能强大的开源空间数据库。它是在1986年诞生于加州大学伯克利分校。PostGIS是PostgreSQL（对象-关系型数据库管理系统）的一个扩展。它支持所有的空间数据类型与一系列重要的GIS 函数，包括完全的OpenGIS 支持、拓扑结构和用于查看、编辑GIS 数据桌面用户相关工具和基础网络访问工具。作为PostgreSQL对象关系数据库系统的扩展模块，PostGIS 支持GIS 空间数据的存储，PostGIS 遵循OGC 的Simple Feature for SQL。PostGIS在PostgreSQL基础上增加了存储空间数据的能力，与Oracle中Spatial相似。PostGIS也具有大型数据库的特性，如数据备份，数据库恢复，灾难恢复等。

​		PostGIS支持所有的空间数据类型，这些类型包括：点(POINT)、线(LINESTRING)、 多边形(POLYGON)、多点(MULTIPOINT)、 多线(MULTILINESTRING)、 多多边形(MULTIPOLYGON)和集合对象集(GEOMETRYCOLLECTION)等。PostGIS支持所有的对象表达方法，比如WKT和WKB。PostGIS支持所有的数据存取和构造方法，如GeomFromText()、AsBinary()，以及GeometryN()等。PostGIS提供简单的空间分析函数(如Area和Length)同时也提供其他一些具有复杂分析功能的函数，比如Distance。PostGIS提供了对于元数据的支持，如GEOMETRYCOLUMNS和SPATIAL REF SYS，同时，PostGIS也提供了相应的支持函数，如AddGeometryColumn和DropGeometryColumn。PostGIS提供了一系列的二元谓词(如Contains、Within、Overlaps和Touches)用于检测空间对象之间的空间关系，同时返回布尔值来表征对象之间符合这个关系。PostGIS提供了空间操作符(如Union和Difference)用于空间数据操作。比如，Union操作符融合多边形之间的边界。两个交迭的多边形通过Union运算就会形成一个新的多边形，这个新的多边形的边界为两个多边形中最大边界。

### 4、WPS

​		Web处理服务（WPS）是用于发布地理空间过程、算法和计算的OGC服务。WPS服务作为geoserver的扩展提供了用于数据处理和地理空间分析的执行操作。

​		默认情况下，WPS不是GeoServer的一部分，但可以作为扩展名使用。

​		与独立的WPS相比，GeoServer WPS的主要优势是 **直接集成** 与其他GeoServer服务和数据目录一起使用。这意味着可以基于在geoserver中提供的数据创建流程，而不是在请求中发送整个数据源。也可以将进程的结果存储为geoserver目录中的新层。通过这种方式，WPS充当一个完整的远程地理空间分析工具，能够从GeoServer读取和写入数据。

<img src="https://live.osgeo.org/archive/10.5/_images/wps13.jpg" alt="WPS in Context" style="zoom: 67%;" />

## 二、环境配置

### 1、GeoServer

https://zhuanlan.zhihu.com/p/73293461

最好使用tomcat+war安装

### 2、PostgreSQL

https://www.postgresql.org/download/注意不能使用远程桌面安装

### 3、PostGIS

使用PostgreGIS中的stack build安装时会卡住，建议从http://download.osgeo.org/postgis/windows中下载安装，需要注意与PostgreSQL版本对应。

### 4、WPS

https://www.osgeo.cn/geoserver-user-manual/services/wps/install.html

## 三、WPS操作

wps为发现和执行地理空间过程定义了三个操作。操作包括：

- GetCapabilities
- DescribeProcess
- 执行

### GetCapabilities

这个 **GetCapabilities** 操作请求服务提供的详细信息，包括服务元数据和描述可用进程的元数据。响应是一个名为 **能力文档** .

与所有ogc getcapabilities请求一样，所需的参数是 `service=WPS` ， `version=1.0.0` 和 `request=GetCapabilities` 。

getCapabilities请求的一个示例是：

```
http://localhost:8080/geoserver/ows?
  service=WPS&
  version=1.0.0&
  request=GetCapabilities
```

### DescribeProcess

这个 **DescribeProcess** 操作请求对服务中可用的WPS进程的描述。参数 `identifier` 指定要描述的进程。可以请求多个进程，用逗号分隔（例如， `identifier=JTS:buffer,gs:Clip` ）。必须至少指定一个进程。

响应是一个XML文档，其中包含有关每个请求进程的元数据，包括以下内容：

- 流程名称、标题和摘要
- 对于每个输入和输出参数：标识符、标题、摘要、多重性和支持的数据类型和格式

流程的示例请求 `JTS:buffer` 是：

```
http://localhost:8080/geoserver/ows?
  service=WPS&
  version=1.0.0&
  request=DescribeProcess&
  identifier=JTS:buffer
```

响应XML文档包含以下信息：

![image-20200815104504264](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200815104504264.png)

### 执行

这个 **执行** 操作是使用指定的输入值和必需的输出数据项执行进程的请求。请求可以作为GET URL或带有XML请求文档的POST发出。因为请求具有复杂的结构，所以更常用的是post表单。

请求所需的输入和输出取决于正在执行的进程。geoserver提供了多种处理几何图形、特征和覆盖范围数据的过程。

下面是一个例子 `Execute` 发布请求。示例流程 (`JTS:buffer` ）将几何图形作为输入 `geom` （在这种情况下，重点是 `POINT(0 0)` a） `distance` （带值 `10` ）量化因子 `quadrantSegments` （这里设为1），以及 `capStyle` （指定为 `flat` ）这个 `<ResponseForm>` 元素指定单个输出的格式 `result` 应为GML 3.1.1。

```
<?xml version="1.0" encoding="UTF-8"?>
<wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:buffer</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>geom</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="application/wkt"><![CDATA[POINT(0 0)]]></wps:ComplexData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>distance</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>10</wps:LiteralData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>quadrantSegments</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>1</wps:LiteralData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>capStyle</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>flat</wps:LiteralData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput mimeType="application/gml-3.1.1">
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

进程使用提供的输入执行缓冲区操作，并返回指定的输出。请求的响应是：

```
<?xml version="1.0" encoding="utf-8"?>
<gml:Polygon xmlns:sch="http://www.ascc.net/xml/schematron"
 xmlns:gml="http://www.opengis.net/gml"
 xmlns:xlink="http://www.w3.org/1999/xlink">
  <gml:exterior>
    <gml:LinearRing>
      <gml:posList>
        10.0 0.0
        0.0 -10.0
        -10.0 0.0
        0.0 10.0
        10.0 0.0
      </gml:posList>
    </gml:LinearRing>
  </gml:exterior>
</gml:Polygon>
```

## 四、Turf.js作为WPs备选方案

Turf 使用 JavaScript 编写，通过 npm 进行包管理。良好的模块化设计使得 Turf 不仅可用于浏览器端，还可以通过 Node.js 在服务器端使用。（感受到了 Node.js 社区一直提倡的前端后台一体化）。

Turf 原生支持 GeoJSON 矢量数据。GeoJSON 的优点是结构简单，并且得到了所有网页地图API的支持；但 GeoJSON 不支持空间索引，这个缺点可能会限制 Turf 处理大型文件的能力效率。

Turf 可以非方便地集成到 Leaflet.js 地图控件中，Mapbox 也为其提供了相应的 Mapbox.js 插件（可以在 mapbox.com 上发布的地图中支持空间分析？）。

Turf 适用于轻量级的 Web GIS 应用，这里的轻量级是指数据量上的轻量，而不是功能上的轻量。

以往的 WebGIS 应用中，空间分析往往由服务器端调用空间数据库完成分析过程，再将结果作为图层返回到浏览器端。这样的流程使得浏览器端的地图应用局限于图层展示与简单的查询。

浏览器端支持空间分析的意义在于，通过网页地图的不仅可提供地名搜索与路径查询（目前 Google Maps 的功能其实与十年前并没有太大区别），而且可以在浏览器中分享空间分析模型。以前的 WebGIS 功能当然也支持空间分析，但是分析过程需要在服务器端进行，本地能够进行的设置有限，现在使用 Turf.js 可以将分析过程完全移到本地，如果页面中提供了参数设置的话，可以在本地对模型进行修改并立即看到分析结果。这样的直接好处有两个方面：更渲的数据展示，以及更加复杂的用户交互（复杂交互本身需要空间分析作为基础）。


 js版本下载地址：https://unpkg.com/@turf/turf@5.1.6/
 官网文档：http://turfjs.org/

## 五、JSTS

JSTS是一个符合OGC规范的简单要素空间位置判定函数JavaScript库，JSTS也是Java类库JTS的一个接口，且与OpenLayer3具有互操作性。
目前原生的OpenLayers3并不支持空间拓扑关系查询，此类库可以作为重要的补充。通过此类库，可以判断多种空间几何的位置关系，最初建立这个工程的目的是为web地图应用提供一套完整的类库来处理和分析简单几何体，但jsts也可以作为一个独立的几何库。
 JSTS是利用原JTS java源码通过AST AST自动翻译转换而成并保持了该API，除了对IO相关的部分类进行了选择性的并手动移植使其支持WKT，GeoJSON和OpenLayers 3。







