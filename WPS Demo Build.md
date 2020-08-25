

# WPS Demo Build

1、确定toop:states边界

<img src="C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200819160610702.png" alt="image-20200819160610702" style="zoom: 67%;" />

<img src="C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200819160645687.png" alt="image-20200819160645687" style="zoom:50%;" />

```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>gs:Bounds</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>features</ows:Identifier>
      <wps:Reference mimeType="text/xml" xlink:href="http://geoserver/wfs" method="POST">
        <wps:Body>
          <wfs:GetFeature service="WFS" version="1.0.0" outputFormat="GML2" xmlns:topp="http://www.openplans.org/topp">
            <wfs:Query typeName="topp:states"/>
          </wfs:GetFeature>
        </wps:Body>
      </wps:Reference>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput>
      <ows:Identifier>bounds</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<ows:BoundingBox xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:ows="http://www.opengis.net/ows/1.1" crs="EPSG:4326">
    <ows:LowerCorner>-124.73142200000001 24.955967</ows:LowerCorner>
    <ows:UpperCorner>-66.969849 49.371735</ows:UpperCorner>
</ows:BoundingBox>
```

2、重新投影Geoserver上的图层

<img src="C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200819161543829.png" alt="image-20200819161543829" style="zoom: 67%;" />



![image-20200819161625284](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200819161625284.png)

```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>vec:Reproject</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>features</ows:Identifier>
      <wps:Reference mimeType="text/xml" xlink:href="http://geoserver/wfs" method="POST">
        <wps:Body>
          <wfs:GetFeature service="WFS" version="1.0.0" outputFormat="GML2" xmlns:tiger="http://www.census.gov">
            <wfs:Query typeName="tiger:giant_polygon"/>
          </wfs:GetFeature>
        </wps:Body>
      </wps:Reference>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>forcedCRS</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>EPSG:4326</wps:LiteralData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>targetCRS</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>EPSG:2326</wps:LiteralData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput mimeType="application/json">
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

```
{
    "type": "FeatureCollection",
    "crs": {
        "type": "name",
        "properties": {
            "name": "EPSG:2326"
        }
    },
    "features": [
        {
            "type": "Feature",
            "geometry": {
                "type": "MultiPolygon",
                "coordinates": [
                    [
                        [
                            [
                                836496.1844,
                                -11651401.7382
                            ],
                            [
                                836368.3267,
                                8352802.7341
                            ],
                            [
                                836368.3267,
                                8352802.7341
                            ],
                            [
                                836496.1844,
                                -11651401.7382
                            ],
                            [
                                836496.1844,
                                -11651401.7382
                            ]
                        ]
                    ]
                ]
            },
            "properties": {},
            "id": "giant_polygon.1"
        }
    ]
}
```

3、JTF：Buffer

![image-20200824164256420](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200824164256420.png)

![image-20200824164329348](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200824164329348.png)



```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:buffer</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>geom</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:LineString xmlns:gml="http://www.opengis.net/gml">
        <gml:posList>0.0 0.0 10.0 0.0 10.0 10.0</gml:posList>
</gml:LineString>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>distance</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>2</wps:LiteralData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>capStyle</ows:Identifier>
      <wps:Data>
        <wps:LiteralData>Square</wps:LiteralData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput mimeType="application/json">
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

```
{
    "type": "Polygon",
    "coordinates": [
        [
            [
                8,
                2
            ],
            [
                8,
                10
            ],
            [
                8,
                12
            ],
            [
                12,
                12
            ],
            [
                12,
                0
            ],
            [
                11.9616,
                -0.3902
            ],
            [
                11.8478,
                -0.7654
            ],
            [
                11.6629,
                -1.1111
            ],
            [
                11.4142,
                -1.4142
            ],
            [
                11.1111,
                -1.6629
            ],
            [
                10.7654,
                -1.8478
            ],
            [
                10.3902,
                -1.9616
            ],
            [
                10,
                -2
            ],
            [
                0,
                -2
            ],
            [
                -2,
                -2
            ],
            [
                -2,
                2
            ],
            [
                8,
                2
            ]
        ]
    ]
}
```

JTF：geometryType



<img src="C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825100347360.png" alt="image-20200825100347360" style="zoom:67%;" />







![image-20200825100416775](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825100416775.png)

```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:geometryType</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>geom</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:LineString xmlns:gml="http://www.opengis.net/gml">
        <gml:posList>0.0 0.0 10.0 0.0 10.0 10.0</gml:posList>
</gml:LineString>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput>
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

JTS：area

![image-20200825102306739](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825102306739.png)

![image-20200825102357757](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825102357757.png)

JTS：boundary

![image-20200825102926154](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825102926154.png)

![image-20200825103031620](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825103031620.png)

```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:boundary</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>geom</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
<gml:exterior>
<gml:LinearRing>
<gml:posList>-180 -90 -180 90 180 90 180 -90 -180 -90</gml:posList>
</gml:LinearRing>
</gml:exterior>
</gml:Polygon>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput mimeType="application/json">
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

```
{
    "type": "LineString",
    "coordinates": [
        [
            -180,
            -90
        ],
        [
            -180,
            90
        ],
        [
            180,
            90
        ],
        [
            180,
            -90
        ],
        [
            -180,
            -90
        ]
    ]
}
```

JTS：centroid

<img src="C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825105601193.png" alt="image-20200825105601193" style="zoom:67%;" />



![image-20200825105625558](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825105625558.png)



```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:centroid</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>geom</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
<gml:exterior>
<gml:LinearRing>
<gml:posList>-180 -90 -180 90 180 90 180 -90 -180 -90</gml:posList>
</gml:LinearRing>
</gml:exterior>
</gml:Polygon>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput mimeType="application/json">
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

```
{
    "type": "Point",
    "coordinates": [
        0,
        0
    ]
}
```

JTS：contain

![image-20200825111328630](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825111328630.png)



![image-20200825142259273](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825142259273.png)



```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:contains</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>a</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
<gml:exterior>
<gml:LinearRing>
<gml:posList>-180 -90 -180 90 180 90 180 -90 -180 -90</gml:posList>
</gml:LinearRing>
</gml:exterior>
</gml:Polygon>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>b</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Point xmlns:gml="http://www.opengis.net/gml">
<gml:pos>-74.01083751 40.70754684</gml:pos>
</gml:Point>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput>
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

JTS：convert

![image-20200825142543107](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825142543107.png)



![image-20200825142607834](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825142607834.png)

```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:convexHull</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>geom</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
<gml:exterior>
<gml:LinearRing>
<gml:posList>-180 -90 -180 90 180 90 180 -90 -180 -90</gml:posList>
</gml:LinearRing>
</gml:exterior>
</gml:Polygon>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput mimeType="application/json">
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

```
{
    "type": "Polygon",
    "coordinates": [
        [
            [
                -180,
                -90
            ],
            [
                -180,
                90
            ],
            [
                180,
                90
            ],
            [
                180,
                -90
            ],
            [
                -180,
                -90
            ]
        ]
    ]
}
```

JTF：crosses

![image-20200825143454564](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825143454564.png)

![image-20200825143534737](C:\Users\乔勇\AppData\Roaming\Typora\typora-user-images\image-20200825143534737.png)

```
<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
  <ows:Identifier>JTS:crosses</ows:Identifier>
  <wps:DataInputs>
    <wps:Input>
      <ows:Identifier>a</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
<gml:exterior>
<gml:LinearRing>
<gml:posList>-180 -90 -180 90 180 90 180 -90 -180 -90</gml:posList>
</gml:LinearRing>
</gml:exterior>
</gml:Polygon>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
    <wps:Input>
      <ows:Identifier>b</ows:Identifier>
      <wps:Data>
        <wps:ComplexData mimeType="text/xml; subtype=gml/3.1.1">
          <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
<gml:exterior>
<gml:LinearRing>
<gml:posList>-73.996035 40.730647 -73.996449 40.72999 -73.997356 40.730437 -73.998047 40.730834 -73.99876 40.731166 -73.999559 40.73158 -73.999079 40.732188 -73.998557 40.732795 -73.996937 40.731984 -73.99549 40.731304 -73.996035 40.730647</gml:posList>
</gml:LinearRing>
</gml:exterior>
</gml:Polygon>

        </wps:ComplexData>
      </wps:Data>
    </wps:Input>
  </wps:DataInputs>
  <wps:ResponseForm>
    <wps:RawDataOutput>
      <ows:Identifier>result</ows:Identifier>
    </wps:RawDataOutput>
  </wps:ResponseForm>
</wps:Execute>
```

