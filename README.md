# gLayers.Leaflet
generic  map layers for leaflet 0.7 and 1.0-rc, moving from original GIST here https://gist.github.com/Sumbera/11114288 to this repo.

L.CanvasLayer  - full screen generic canvas layer for leaflet

##API

|methods       | description            | 
| ------------- |:-------------| 
|needRedraw   | will schedule next frame call for drawLayer|
|delegate(object)   | optionaly set receiver of the events if not 'inheriting' from L.CanvasLayer |


events          | description            | 
| ------------- |:-------------| 
|onLayerDidMount   | after layer is attached/added to the map|
|onLayerWillUnmount   | before  layer is removed from the map|
|onDrawLayer(info)       | when layer is drawn , info contains view parameters like bounds, size, canvas etc.
note :  events will be called only if presented on the 'subclass' or if delegate(receiver) is set

##  Examples 
### Basic usage
```javascript
        L.canvasLayer()
            .delegate(this) // -- if we do not inherit from L.CanvasLayer  we can setup a delegate to receive events from L.CanvasLayer
            .addTo(leafletMap);
      
        function onDrawLayer(info) {
            var ctx = info.canvas.getContext('2d');
            ctx.clearRect(0, 0, info.canvas.width, info.canvas.height);
            ctx.fillStyle = "rgba(255,116,0, 0.2)";
            for (var i = 0; i < data.length; i++) {
                var d = data[i];
                if (info.bounds.contains([d[0], d[1]])) {
                    dot = info.layer._map.latLngToContainerPoint([d[0], d[1]]);
                    ctx.beginPath();
                    ctx.arc(dot.x, dot.y, 3, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.closePath();
                }
            }
        };
```

### Custom layer
```javascript
  
          myCustomCanvasDraw= function(){
              this.onLayerDidMount = function (){      
                 // -- prepare custom drawing    
              };
              this.onLayerWillUnmount  = function(){
                 // -- custom cleanup    
              };
              this.setData = function (data){
                // -- custom data set
                this.needRedraw(); // -- call to drawLayer
              };
              this.onDrawLayer = function (viewInfo){
              // -- custom  draw
              }
              
          }
          
          myCustomCanvasDraw.prototype = new L.CanvasLayer(); // -- setup prototype 
          
          var myLayer = new myCustomCanvasDraw();
          myLayer.addTo(leafletMap);
 ```   


Other useful full view  Leaflet Canvas sources here:
- [leaflet.heat](https://github.com/Leaflet/Leaflet.heat)
- [Full Canvas] (https://github.com/cyrilcherian/Leaflet-Fullcanvas)

