# Bigmap

- 开发文档：http://www.bigemap.com/offlinemaps/api/#map-example

- 什么是离线地图
  - 在没有网络的情况下，需要用到地图，搭建一个自己的地图服务器，这就叫做离线地图（局域网搭建地图服务器）
  - 如果将离线地图放在公网上面，它就是有线地图
  - 简单来说，离线地图就是我们自己搭建的一个地图服务器，跟百度、高德、谷歌的地图服务器一样，可以通过地址来访问，也可以通过调用服务器上提供的接口来进行二次开发
  - 瓦片地图
    - 瓦片地图金字塔模型是一种多分辨率[层次模型](https://baike.baidu.com/item/层次模型/3164933)，从瓦片金字塔的底层到顶层，分辨率越来越低，但表示的地理范围不变。首先确定地图服务平台所要提供的缩放级别的数量N，把缩放级别最高、地图比例尺最大的地图图片作为金字塔的底层，即第0层，并对其进行分块，从地图图片的左上角开始，从左至右、从上到下进行切割，分割成相同大小(比如256x256像素)的正方形地图瓦片，形成第0层瓦片矩阵;在第0层地图图片的基础上，按每像素分割为2×2个像素的方法生成第1层地图图片，并对其进行分块，分割成与下一层相同大小的正方形地图瓦片，形成第1层瓦片矩阵;采用同样的方法生成第2层瓦片矩阵;…;如此下去，直到第N一1层，构成整个瓦片金字塔。
  
- 离线地图服务器环境搭建及配置
  - BIGEMAP离线地图服务器下载地址：http://www.bigemap.com/offlinemaps/
  - 默认9000端口，如果想要修改端口，可以在`C盘->用户->ZZY->.bm-server->app.json`中修改端口
  - 选择在线服务，下面的地图二维、三维可以预览，点击开发使用可以查看用法
  
  
  
- 如何导入地图数据
  
  - Bigemap GIS Office下载地址：http://www.bigemap.com/reader/download/
  - 打开Bigemap GIS Office，右上角选择行政区域，点击下载
  - 下载对话框中，存储选项选择`瓦片:BIGEMAP`，选择级别，然后确定
  - 然后到Bigemap离线地图服务器中，选择**添加离线地图（2D）**
  - 填写服务名，导入瓦片文件，然后创建
  
- 添加高程数据来实现地表起伏三位效果
  - 像上面的导入地图数据一样，但是在下载的时候要在下载对话框顶部选择`高程（等高线）`
  - 注意`存储格式`要选择`BM离线地图高程库（*.bmele）`
  - 然后到Bigemap离线地图服务器中，选择**添加离线地图（3D）**
  - 填写服务名，导入文件，然后创建
  
- 呈现三维立体建筑物效果
  - 选择某个行政区域，双击。
  
  - 在下载对话框，选择建筑轮廓
  
  - 下方存储格式选择`矢量建筑瓦片（*.vmbtiles）`
  
  - 然后到Bigemap离线地图服务器中，选择**添加矢量瓦片**
  
  - 填写服务名，导入文件，然后创建
  





### 地图使用

- 添加点

  -  创建点

    | 构造                                                     | 描述                                         |
    | :------------------------------------------------------- | :------------------------------------------- |
    | `BM.marker( <LatLng> latlng, <Marker options> options?)` | 给定地理点和可选的选项对象实例化Marker对象。 |

  - 将其添加进地图中

    ```js
    var marker=BM.marker(map.getCenter()).addTo(map);
    //map.getCenter()就是将点设置在当前地图的中心坐标上
    //addTo(map)操作就是将其添加进地图中，map是容器的id
    ```

  - 让点可以拖动

    - 为点设置可拖拽属性

      ```js
       var marker=BM.marker(map.getCenter(),{draggable:true}).addTo(map);
      ```

  - 为点添加标注

    - ```js
      //添加一个简单的提示 详细API请参见 http://www.bigemap.com/offlinemaps/api/#popup
      marker.bindTooltip("my tooltip text").openTooltip();
      ```

    - bindTooltip后的参数为想要显示的标注文本

      

- 添加线

  - 创建线

    | 构造                                                         | 描述                                                         |
    | :----------------------------------------------------------- | :----------------------------------------------------------- |
    | `BM.polyline( <LatLng[]> latlngs, <Polyline options> options?)` | 在给定地理点数组和可选的选项对象的情况下实例化折线对象。您可以通过传递地理点数组的数组来创建[`Polyline`](http://www.bigemap.com/offlinemaps/api/#polyline)具有多个单独行（`MultiPolyline`）的对象。 |

  - lating是一个数组，表示线段上多个点的坐标

    第二个参数可以配置线段的颜色

    最后要让地图适配线段

    ```js
    //线段的坐标点
            var latlngs = [
                [23, 113.68],
                [24, 108],
                [26, 118.2]
            ];
            //创建线段，并设置颜色为红色，具体请参见 :http://www.bigemap.com/offlinemaps/api/#polyline
            var polyline = BM.polyline(latlngs, {color: 'red'}).addTo(map);
    
            // 让地图适配当前的线段
            map.fitBounds(polyline.getBounds());
    ```

    

- 监听地图的点击事件

  - 为实例map添加onclick事件

  - ```js
    //为地图添加一个单击事件，更多事件列表请参见 ：http://www.bigemap.com/offlinemaps/api/#map-baselayerchange
            map.on('click',function(e){
                var str='lat:'+e.latlng.lat+',lng:'+e.latlng.lng;
                alert(str);
            });
    ```

- 地图的各种事件

  - | 事件          | 数据            | 描述                                                         |
    | :------------ | :-------------- | :----------------------------------------------------------- |
    | `click`       | `MouseEvent`    | 用户单击（或点击）地图时触发。                               |
    | `dblclick`    | `MouseEvent`    | 当用户双击（或双击）地图时触发。                             |
    | `mousedown`   | `MouseEvent`    | 当用户在地图上按下鼠标按钮时触发。                           |
    | `mouseup`     | `MouseEvent`    | 当用户在地图上释放鼠标按钮时触发。                           |
    | `mouseover`   | `MouseEvent`    | 鼠标进入地图时触发。                                         |
    | `mouseout`    | `MouseEvent`    | 鼠标离开地图时触发。                                         |
    | `mousemove`   | `MouseEvent`    | 鼠标在地图上移动时触发。                                     |
    | `contextmenu` | `MouseEvent`    | 当用户在地图上按下鼠标右键时触发，防止默认浏览器上下文菜单显示此事件是否有侦听器。当用户持续一次触摸（也称为长按）时，也会在手机上触发。 |
    | `keypress`    | `KeyboardEvent` | 当用户在聚焦地图时从键盘按下键时触发。                       |
    | `preclick`    | `MouseEvent`    | 鼠标在地图上单击之前触发（有时在任何现有点击处理程序开始运行之前需要点击某些内容时有用）。 |

  - 例子：鼠标右键时为地图添加一个点

    ```js
     map.on('contextmenu',function(e){
                BM.marker(e.latlng).addTo(this);
            });
    ```

  - **注：给点和线添加事件也跟给地图添加事件一样，只不过id改变了而已**

- 监听地图的缩放事件

  - 为地图添加一个`zoomend`事件

  - ```js
     //html
    <div id="zoom">
        当前级别 : 2
    </div>
    
    
    //js
    map.on('zoomend',function(e){
            document.getElementById('zoom').innerHTML='当前级别 : '+ this.getZoom();
        });
    ```

  - this.getZoom()获取当前层级

- 监听地图的移动事件

  - 为地图添加一个`moveend`事件

  - ```js
    //html
    <div id="move">
        当前中心点: 纬度:0.00000,经度:0.00000
    </div>
    
    //js
    map.on('moveend',function(e){
            var c=this.getCenter();
            document.getElementById('move').innerHTML='当前中心点: 纬度:'+c.lat.toFixed(5)+',经度:'+c.lng.toFixed(5);
        });
    ```

    

- 编辑图形

  - 让图形可以编辑

    - 设置属性`editing.enable()`

    - 如：

      ```js
      polyline.editing.enable();
      ```

  - 让图形不能编辑

    - 设置属性`editing.disable()`

    - 如：

      ```js
      polyline.editing.disable()
      ```

  - 监听编辑事件

    - 监听`BM.Draw.Event.EDITVERTEX`事件

    - ```js
      map.on(BM.Draw.Event.EDITVERTEX,function (e){
              console.log('edit');
              console.log(e.poly);
         })
      ```

      

- ### 层、图层组、窗格的概念

  - #### 层

    - 是bm图层使用的Layer基类中的一组方法。继承所有方法，选项和事件

    - 用法：

      ```js
      var layer = BM.Marker(latlng).addTo(map);
      //latlng就是经纬度，要配置选项的话就是：BM.Marker(latlng，{配置})
      layer.addTo(map); 
      layer.remove();
      ```

    - 配置选项：

      - | 配置          | 类型     | 默认            | 描述                                                         |
        | :------------ | :------- | :-------------- | :----------------------------------------------------------- |
        | `pane`        | `String` | `'overlayPane'` | 默认情况下，图层将添加到地图的[叠加窗格中](http://www.bigemap.com/offlinemaps/api/#map-overlaypane)。覆盖此选项将导致默认情况下将图层放置在另一个窗格上。 |
        | `attribution` | `String` | `null`          | 要在归属控件中显示的字符串，描述图层数据，例如“©Mapbox”。    |

        

    - ##### 层的事件

      - | 事件     | 数据    | 描述                   |
        | :------- | :------ | :--------------------- |
        | `add`    | `Event` | 将图层添加到地图后触发 |
        | `remove` | `Event` | 从地图中删除图层后触发 |

      - 弹出事件

        | 事件         | 数据         | 描述                             |
        | :----------- | :----------- | :------------------------------- |
        | `popupopen`  | `PopupEvent` | 打开绑定到此图层的弹出窗口时触发 |
        | `popupclose` | `PopupEvent` | 绑定到此图层的弹出窗口关闭时触发 |

      - 工具提示事件

        | 事件           | 数据           | 描述                               |
        | :------------- | :------------- | :--------------------------------- |
        | `tooltipopen`  | `TooltipEvent` | 打开绑定到此图层的工具提示时触发。 |
        | `tooltipclose` | `TooltipEvent` | 绑定到此图层的工具提示关闭时触发。 |

    - ##### 层的方法

      - | 方法                           | 返回          | 描述                                                         |
        | :----------------------------- | :------------ | :----------------------------------------------------------- |
        | `addTo( <Map|LayerGroup> map)` | `this`        | 将图层添加到给定的地图或图层组。                             |
        | `remove()`                     | `this`        | 从当前处于活动状态的地图中删除图层。                         |
        | `removeFrom( <Map> map)`       | `this`        | 从给定的地图中删除图层                                       |
        | `getPane( <String> name?)`     | `HTMLElement` | 返回`HTMLElement`表示地图上命名窗格的内容。如果`name`省略，则返回此图层的窗格。 |
        | `getAttribution()`             | `String`      | 由the使用`attribution control`，返回[归属选项](http://www.bigemap.com/offlinemaps/api/#gridlayer-attribution)。 |

        

  - #### 图层组(Layer Group)

    - 用于将多个图层分组并将其作为一个图层处理。如果将其添加到地图中，则还会在地图上添加/删除从组中添加或删除的任何图层。延伸[`Layer`](http://www.bigemap.com/offlinemaps/api/#layer)。

    - 用法：

      ```js
      BM.layerGroup([marker1, marker2])
          .addLayer(polyline)
          .addTo(map);
      ```

    - 创建：

      | 构造                                                   | 描述                                                        |
      | :----------------------------------------------------- | :---------------------------------------------------------- |
      | `BM.layerGroup( <Layer[]> layers?, <Object> options?)` | 创建一个图层组，可选地给出一组初始图层和一个`options`对象。 |

    - 示例：

      ```js
      var marker1 = BM.marker(map.getCenter(),{draggable:true});
      var marker2 = BM.marker(map.getCenter(),{draggable:true});
      var latlngs = [
                  [23, 113.68],
                  [24, 108],
                  [26, 118.2]
              ];
      //创建线段，并设置颜色为红色
      var polyline = BM.polyline(latlngs, {color: 'red'});
      var group = new BM.layerGroup([marker1,marker2]).addLayer(polyline).addTo(map)
      ```

    - ##### 选项和事件都与上面的层相同

    - 方法：

      - | 方法                                           | 返回      | 描述                                                         |
        | :--------------------------------------------- | :-------- | :----------------------------------------------------------- |
        | `toGeoJSON()`                                  | `Object`  | 返回一个 [`GeoJSON`](http://www.bigemap.com/offlinemaps/api/#geojson)层组的表示（作为以GeoJSON `FeatureCollection`，`GeometryCollection`或`MultiPoint`）。 |
        | `addLayer( <Layer> layer)`                     | `this`    | 将给定图层添加到组中。                                       |
        | `removeLayer( <Layer> layer)`                  | `this`    | 从组中删除给定的图层。                                       |
        | `removeLayer( <Number> id)`                    | `this`    | 从组中删除具有给定内部ID的图层。                             |
        | `hasLayer( <Layer> layer)`                     | `Boolean` | 返回`true`如果给定的层，目前添加到该组。                     |
        | `hasLayer( <Number> id)`                       | `Boolean` | 返回`true`如果给定的内部ID目前添加到该组。                   |
        | `clearLayers()`                                | `this`    | 从组中删除所有图层。                                         |
        | `invoke( <String> methodName, …)`              | `this`    | 调用`methodName`此组中包含的每个层，传递任何其他参数。如果包含的图层未实现，则无效`methodName`。 |
        | `eachLayer( <Function> fn, <Object> context?)` | `this`    | 迭代组的层，可选地指定迭代器函数的上下文。`group.eachLayer(function (layer) {    layer.bindPopup('Hello'); }); ` |
        | `getLayer( <Number> id)`                       | `Layer`   | 返回具有给定内部ID的图层。                                   |
        | `getLayers()`                                  | `Layer[]` | 返回添加到组中的所有图层的数组。                             |
        | `setZIndex( <Number> zIndex)`                  | `this`    | 调用`setZIndex`该组中包含的每个层，传递z-index。             |
        | `getLayerId( <Layer> layer)`                   | `Number`  | 返回图层的内部ID                                             |

      

  - #### Feature Group

    - 扩展[`LayerGroup`](http://www.bigemap.com/offlinemaps/api/#layergroup)使得更容易对其所有成员层执行相同的操作：

      - [`bindPopup`](http://www.bigemap.com/offlinemaps/api/#layer-bindpopup)一次将弹出窗口绑定到所有图层（同样如此[`bindTooltip`](http://www.bigemap.com/offlinemaps/api/#layer-bindtooltip)）
      - 事件传播到[`FeatureGroup`](http://www.bigemap.com/offlinemaps/api/#featuregroup)，因此如果组具有事件处理程序，它将处理来自任何层的事件。这包括鼠标事件和自定义事件。
      - 有`layeradd`和`layerremove`事件

    - 用法：

      ```js
      BM.featureGroup([marker1, marker2, polyline])
          .bindPopup('Hello world!')
          .on('click', function() { alert('Clicked on a member of the group!'); })
          .addTo(map);
      ```

    - 示例：

      ```js
       var marker1 = BM.marker(map.getCenter(),{draggable:true});
              var marker2 = BM.marker(map.getCenter(),{draggable:true});
              var latlngs = [
                  [23, 113.68],
                  [24, 108],
                  [26, 118.2]
              ];
              //创建线段，并设置颜色为红色，具体请参见 :http://www.bigemap.com/offlinemaps/api/#polyline
              var polyline = BM.polyline(latlngs, {color: 'red'});
      
              var group = new BM.featureGroup([marker1,marker2,polyline])
              .bindPopup('Hello world!')
              .on('click', function() { alert('Clicked on a member of the group!'); })
              .addTo(map)
      ```

    - 创建

      - | 构造                                 | 描述                                         |
        | :----------------------------------- | :------------------------------------------- |
        | `BM.featureGroup( <Layer[]> layers)` | 创建一个要素组，可选择为其提供一组初始图层。 |

    - 事件

      - | 事件          | 数据         | 描述                                                         |
        | :------------ | :----------- | :----------------------------------------------------------- |
        | `layeradd`    | `LayerEvent` | 将图层添加到此时触发 [`FeatureGroup`](http://www.bigemap.com/offlinemaps/api/#featuregroup) |
        | `layerremove` | `LayerEvent` | 从此处移除图层时触发 [`FeatureGroup`](http://www.bigemap.com/offlinemaps/api/#featuregroup) |

    - 方法

      - | 方法                              | 返回           | 描述                                                     |
        | :-------------------------------- | :------------- | :------------------------------------------------------- |
        | `setStyle( <Path options> style)` | `this`         | 将给定路径选项设置为具有`setStyle`方法的组的每个层。     |
        | `bringToFront()`                  | `this`         | 将图层组置于所有其他图层的顶部                           |
        | `bringToBack()`                   | `this`         | 将图层组带到所有其他图层的背面                           |
        | `getBounds()`                     | `LatLngBounds` | 返回要素组的LatLngBounds（根据其子项的边界和坐标创建）。 |

      

    

  - #### 窗格

    - 窗格：窗格是用于控制地图上图层排序的DOM元素。可以使用[`map.getPane`](http://www.bigemap.com/offlinemaps/api/#map-getpane)或 [`map.getPanes`](http://www.bigemap.com/offlinemaps/api/#map-getpanes)方法访问窗格。可以使用该[`map.createPane`](http://www.bigemap.com/offlinemaps/api/#map-createpane)方法创建新窗格 。

    - 每个地图都有以下默认窗格，这些窗格仅在zIndex中有所不同。

      | 窗格          | 类型          | Z-指数   | 描述                                                         |
      | :------------ | :------------ | :------- | :----------------------------------------------------------- |
      | `mapPane`     | `HTMLElement` | `'auto'` | 包含所有其他地图窗格的窗格                                   |
      | `tilePane`    | `HTMLElement` | `200`    | 适用于[`GridLayer`](http://www.bigemap.com/offlinemaps/api/#gridlayer)s和[`TileLayer`](http://www.bigemap.com/offlinemaps/api/#tilelayer)s的窗格 |
      | `overlayPane` | `HTMLElement` | `400`    | 矢量窗格（[`Path`](http://www.bigemap.com/offlinemaps/api/#path)s，[`Polyline`](http://www.bigemap.com/offlinemaps/api/#polyline)s和[`Polygon`](http://www.bigemap.com/offlinemaps/api/#polygon)s），[`ImageOverlay`](http://www.bigemap.com/offlinemaps/api/#imageoverlay)s和[`VideoOverlay`](http://www.bigemap.com/offlinemaps/api/#videooverlay)s |
      | `shadowPane`  | `HTMLElement` | `500`    | 叠加阴影的窗格（例如[`Marker`](http://www.bigemap.com/offlinemaps/api/#marker)阴影） |
      | `markerPane`  | `HTMLElement` | `600`    | 窗格[`Icon`](http://www.bigemap.com/offlinemaps/api/#icon)第[`Marker`](http://www.bigemap.com/offlinemaps/api/#marker)小号 |
      | `tooltipPane` | `HTMLElement` | `650`    | Pane for [`Tooltip`](http://www.bigemap.com/offlinemaps/api/#tooltip)s。 |
      | `popupPane`   | `HTMLElement` | `700`    | Pane for [`Popup`](http://www.bigemap.com/offlinemaps/api/#popup)s。 |

    - 

  - 

- 
