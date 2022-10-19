<template>
  <div class="map-wrap">
    <a href="https://www.maptiler.com" class="watermark"><img
      src="https://api.maptiler.com/resources/logo.svg"
      alt="MapTiler logo"
    ></a>
    <div ref="mapContainer" class="map" />
  </div>
</template>

<script>
import { Map, Marker, NavigationControl } from 'maplibre-gl'
import { shallowRef, onMounted, onUnmounted, markRaw } from 'vue'

export default {
  name: 'GeoMap',
  setup () {
    const mapContainer = shallowRef(null)
    const map = shallowRef(null)

    // eslint-disable-next-line no-unused-expressions
    onMounted(() => {
      const apiKey = 'xGbFINbDbGQiTqjzJJQy'

      const initialState = { lat: 52.370216, lng: 4.895168, zoom: 0 }

      map.value = markRaw(new Map({
        container: mapContainer.value,
        style: `https://api.maptiler.com/maps/streets/style.json?key=${apiKey}`,
        center: [initialState.lng, initialState.lat],
        zoom: initialState.zoom
      }))

      const nav = new NavigationControl()
      map.value.addControl(nav, 'top-left')

      new Marker({ color: '#FF0000' })
        .setLngLat([12.65147, 55.608166])
        .addTo(map.value)
    })

    onUnmounted(() => {
      map.value?.remove()
    })

    return {
      map, mapContainer
    }
  }
}
</script>

<style scoped>
@import '../node_modules/maplibre-gl/dist/maplibre-gl.css';

.map-wrap {
    position: relative;
    width: 100%;
    height: 100vh;
    /* calculate height of the screen minus the heading */
}

.map {
    position: absolute;
    width: 100%;
    height: 100%;
}

</style>
