<template>
  <q-page padding class="flex">
    <q-card style="flex:1" class="col-md-4">
      <!-- <l-map :zoom="zoom" :center="center">
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
          <l-marker :lat-lon="marker" ></l-marker>
      </l-map> -->
      <div id="app" class="container">
        <!-- The rest of the app goes here -->
        <div class="row">
          <div class="col-md-9">
            <!-- The map goes here -->
            <div id="map" class="map" ></div>
          </div>
          <div class="col-md-3">
            <!-- The layer checkboxes go here -->
          </div>
        </div>
      </div>
    </q-card>
    <q-card class="col-lg-3">
      <div class="form-check" v-for="layer in layers" :key="layer.id">
        <!-- label and input elements go here -->
        <label class="form-check-label">
        <input
          class="form-check-input"
          type="checkbox"
          v-model="layer.active"
          @change="layerChanged(layer.id, layer.active)"
        />
        {{ layer.name }}
        </label>
      </div>
      <!-- <div>
        {{ pointsss }}
      </div> -->
    </q-card>
  </q-page>
</template>

<script>
// import { LMap, LTileLayer, LMarker } from 'vue2-leaflet'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

delete L.Icon.Default.prototype._getIconUrl

L.Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png')
})

export default {
  name: 'Map',
  components: {
    // LMap,
    // LTileLayer,
    // LMarker
  },
  methods: {
    latLng: function (lat, lng) {
      return L.latLng(lat, lng)
    },
    initMap () {
      this.map = L.map('map').setView([38.6188362, -90.1947098], 13)
      this.tileLayer = L.tileLayer(
        'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
        {
          maxZoom: 18,
          attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
        }
      )
      this.tileLayer.addTo(this.map)
    },
    initLayers () {
      this.layers.forEach((layer) => {
        const markerFeatures = layer.features.filter(feature => feature.type === 'marker')
        const polygonFeatures = layer.features.filter(feature => feature.type === 'polygon')
        markerFeatures.forEach((feature) => {
          feature.leafletObject = L.marker(feature.coords)
            .bindPopup(feature.name)
        })
        polygonFeatures.forEach((feature) => {
          feature.leafletObject = L.polygon(feature.coords)
            .bindPopup(feature.name)
        })
        layer.features.forEach((feature) => {
        /* Show or hide the feature depending on the active argument */
          if (layer.active) {
            feature.leafletObject.addTo(this.map)
          } else {
            feature.leafletObject.removeFrom(this.map)
          }
        })
      })
    },
    layerChanged (layerId, active) {
      const layer = this.layers.find(layer => layer.id === layerId)
      layer.features.forEach((feature) => {
        /* Show or hide the feature depending on the active argument */
        if (active) {
          feature.leafletObject.addTo(this.map)
        } else {
          feature.leafletObject.removeFrom(this.map)
        }
      })
    }
  },
  mounted () {
    setTimeout(function () { window.dispatchEvent(new Event('resize')) }, 250)
    this.initMap()
    this.initLayers()
  },
  data () {
    return {
      zoom: 13,
      center: L.latLng(47.413220, -1.219482),
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: L.latLng(47.413220, -1.219482),
      pointsss: [
        L.latLng(47.413220, -1.219482),
        L.latLng(47.423221, -1.229483)
      ],
      points: [
        { latitude: 47.413220, longitude: -1.219482 },
        { latitude: 47.423221, longitude: -1.229483 }
      ],
      map: null,
      tileLayer: null,
      layers: [
        {
          id: 0,
          name: 'Restaurants',
          active: true,
          features: [
            {
              id: 0,
              name: 'Bogart\'s Smokehouse',
              type: 'marker',
              coords: [38.6109607, -90.2050322]
            },
            {
              id: 1,
              name: 'Pappy\'s Smokehouse',
              type: 'marker',
              coords: [38.6350008, -90.2261532]
            },
            {
              id: 2,
              name: 'Broadway Oyster Bar',
              type: 'marker',
              coords: [38.6188362, -90.1947098]
            },
            {
              id: 3,
              name: 'Charlie Gitto\'s On the Hill',
              type: 'marker',
              coords: [38.617972, -90.2756436]
            },
            {
              id: 4,
              name: 'Sugarfire',
              type: 'marker',
              coords: [38.6304077, -90.1916921]
            },
            {
              id: 5,
              name: 'The Shaved Duck',
              type: 'marker',
              coords: [38.6036282, -90.2381407]
            },
            {
              id: 6,
              name: 'Mango Restaurant',
              type: 'marker',
              coords: [38.6313642, -90.1961267]
            },
            {
              id: 7,
              name: 'Vessel 1',
              type: 'marker',
              coords: [38.632364, -90.2061267],
              timestamp: Date(Date.UTC(120, 1, 2, 3, 4, 5))
            },
            {
              id: 8,
              name: 'Vessel 2',
              type: 'marker',
              coords: [38.6333642, -90.2161267],
              timestamp: Date(Date.UTC(120, 1, 3, 3, 4, 5))
            }
          ]
        },
        {
          id: 1,
          name: 'City/County Boundaries',
          active: true,
          features: [
            {
              id: 0,
              name: 'City of St. Louis',
              type: 'polygon',
              coords: [
                [38.770547, -90.168056],
                [38.753816, -90.177326],
                [38.747390, -90.183849],
                [38.731456, -90.206337],
                [38.719805, -90.212002],
                [38.706142, -90.210629],
                [38.692879, -90.202217],
                [38.680150, -90.189857],
                [38.665139, -90.182991],
                [38.646774, -90.179729],
                [38.630818, -90.179214],
                [38.615663, -90.183849],
                [38.601713, -90.190201],
                [38.587759, -90.204620],
                [38.577427, -90.219212],
                [38.564140, -90.232258],
                [38.545615, -90.248566],
                [38.531650, -90.257664],
                [38.538901, -90.270023],
                [38.548702, -90.273113],
                [38.561053, -90.294399],
                [38.574072, -90.309334],
                [38.596346, -90.320320],
                [38.614054, -90.314827],
                [38.632159, -90.304527],
                [38.651198, -90.302296],
                [38.664067, -90.293713],
                [38.683768, -90.278263],
                [38.700650, -90.265388],
                [38.717662, -90.253887],
                [38.722349, -90.238266],
                [38.729715, -90.221272],
                [38.742302, -90.203934],
                [38.754886, -90.191746],
                [38.764792, -90.184021],
                [38.771350, -90.183334]
              ]
            }
          ]
        }
      ]
    }
  }
}
</script>
<style>
.map { height: 750px; width: 900px; }
</style>
