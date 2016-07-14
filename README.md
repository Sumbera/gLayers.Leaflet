# LayersIN
generic  map layers for leaflet 0.7 and 1.0-rc

L.CanvasLayer API:

##API:

|methods       | description            | 
| ------------- |:-------------| 
|needRedraw   | will schedule next frame call for drawLayer|


events          | description            | 
| ------------- |:-------------| 
|onLayerDidMount   | after layer is attached/added to the map|
|onLayerWillUnmount   | before  layer is removed from the map|
|onDrawLayer(info)       | when layer is drawn , info contains view parameters like bounds, size, canvas etc.
note :  events will be called only if presented on the 'subclass'

##  example 

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
- [CartoDb Leaflet.Canvas] (https://github.com/CartoDB/Leaflet.CanvasLayer)
