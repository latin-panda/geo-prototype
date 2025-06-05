<script setup lang="ts">
import { Loader } from '@googlemaps/js-api-loader';
import { TerraDraw, TerraDrawSelectMode, TerraDrawPolygonMode, TerraDrawLineStringMode, TerraDrawPointMode, TerraDrawFreehandMode } from 'terra-draw';
import { TerraDrawGoogleMapsAdapter } from 'terra-draw-google-maps-adapter';
import { ref } from 'vue';

const apiKey = import.meta.env.VITE_MAP_API_KEY;

const props = defineProps<{
  data: number[][];
}>();

let draw: TerraDraw | null = null;
const hasAddedPolygon = ref(false);

const loader = new Loader({
  apiKey,
  version: 'weekly',
});

loader.load().then((google) => {
  const mapElement = document.getElementById('geo-prototype-map')

  const map = new google.maps.Map(mapElement, {
    center: { lat: 41.8902, lng: 12.4922 },
    zoom: 17,
    // Selectively disable UI controls instead of disabling all
    mapTypeControl: false,
    streetViewControl: false,
    fullscreenControl: true,
    zoomControl: true,
    // Disable clickable icons (still supported)
    clickableIcons: false,
    // Enable WebGL for better performance (optional)
    mapId: 'YOUR_MAP_ID', // Required for Advanced Markers and WebGL
  });

  // Optional: Add error handling for map initialization
  map.addListener('idle', () => {
    console.log('Map loaded successfully');
    draw = new TerraDraw({
      adapter: new TerraDrawGoogleMapsAdapter({
        lib: google.maps,
        map,
        coordinatePrecision: 9,
      }),
      modes: [
          new TerraDrawSelectMode({
            flags: {
              point: {
                feature: {
                  draggable: true,
                  rotateable: true,
                  scaleable: true,
                  selfIntersectable: true,
                  coordinates: {
                    snappable: true,
                    midpoints: true,
                    draggable: true,
                    deletable: true,
                  },
                },
              },
              polygon: {
                feature: {
                  draggable: true,
                  rotateable: true,
                  scaleable: true,
                  selfIntersectable: true,
                  coordinates: {
                    snappable: true,
                    midpoints: true,
                    draggable: true,
                    deletable: true,
                  },
                },
              },
              linestring: {
                feature: {
                  draggable: true,
                  rotateable: true,
                  scaleable: true,
                  selfIntersectable: true,
                  coordinates: {
                    snappable: true,
                    midpoints: true,
                    draggable: true,
                    deletable: true,
                  },
                },
              },
              freehand: {
                feature: {
                  draggable: true,
                  rotateable: true,
                  scaleable: true,
                  selfIntersectable: true,
                  coordinates: {
                    snappable: true,
                    midpoints: true,
                    draggable: true,
                    deletable: true,
                  },
                },
              },
            },
          }),
        new TerraDrawPolygonMode(),
        new TerraDrawLineStringMode(),
        new TerraDrawPointMode(),
        new TerraDrawFreehandMode(),
      ],
    });

    draw.start();

    // Unlike other adapters, the google adapter 'ready' event is required to be listened for
    // because the adapter creates an OverlayView which is only ready asynchronously
    draw.on('ready', () => {
      if (hasAddedPolygon.value) {
        return;
      }

      if (!props.data?.length) {
        draw?.setMode('polygon');
        return;
      }

      draw?.addFeatures(props.data);
      hasAddedPolygon.value = true;

      draw?.setMode('select');
    });

    draw.on("select", (id: string) => {
      console.log('select', id);
    });
  }, { once: true });

  // Optional: Handle map errors
  map.addListener('error', console.error);
});

defineExpose({
  getGeometries: () => {
    if (draw) {
      return draw.getSnapshot();
    }
    return [];
  },
});
</script>

<template>
<div id="geo-prototype-map"></div>
</template>

<style scoped>
#geo-prototype-map {
  width: 100%;
  height: 500px;
}
</style>
