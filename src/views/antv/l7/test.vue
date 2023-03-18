<template>
  <div id="map"></div>
</template>

<script setup>
import { Scene, PointLayer, HeatmapLayer } from '@antv/l7';
import { GaodeMap,Mapbox  } from '@antv/l7-maps';

const initMap= ()=>{
  const scene = new Scene({
    id: 'map',
    map: new Mapbox({
      style: 'dark',
      pitch: 0,
      center: [ 127.5671666579043, 7.445038892195569 ],
      zoom: 2.632456779444394
    })
  });
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