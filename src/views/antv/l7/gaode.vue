<template>
  <div id="map"></div>
</template>

<script setup>
import { Scene, PointLayer, HeatmapLayer } from '@antv/l7';
import { GaodeMap,Mapbox  } from '@antv/l7-maps';

let scene = reactive({})

const initMap= ()=>{
   scene = new Scene({
    id: 'map',
    map: new GaodeMap({
      // style: 'dark',
      pitch: 30,
      // center: [ 127.5671666579043, 7.445038892195569 ],
      center: [ 120, 30 ],
      zoom: 5
    })
  });

   const layer = new PointLayer();
  layer.source([{
    lng:120,
    lat:30,
    v:30,
  }],{
    parser:{
      type:'json',
      x:'lng',
      y:'lat'
    }
  })
      // .size(30)
      // .size('v',(value)=>value)
      .size([10,10,100])
      // .shape('circle')
      .shape('cylinder')
      // .color('#f00')
      .color('v',['#f00','#ff0'])
      .style({
        opacity:0.5
      })

  scene.on('loaded',()=>{
    scene.addLayer(layer)
  })

}

nextTick(()=>{
  initMap()
})
</script>

<style scoped>
#map{
  width: 100%;
  height: 800px;
}
</style>