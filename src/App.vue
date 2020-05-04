<template>
  <div id="app">
    <Header />
    <div class="columns" style="margin: 2% 5% 0 5%">
      <!-- Leaflet -->
      <div class="column is-9 animated zoomIn delay-1s">
          <div style="height: 100%;">
            <!-- <div class="info" style="height: 15%">
              <span>Center: {{ center }}</span>
              <span>Zoom: {{ zoom }}</span>
              <span>Bounds: {{ bounds }}</span>
            </div> -->

            <l-map
              style="height: 100%; width: 100%"
              :zoom="zoom"
              :center="center"
              @update:zoom="zoomUpdated"
              @update:center="centerUpdated"
              @update:bounds="boundsUpdated"
            >
              <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>

              <l-marker :lat-lng="markerLatLng" ></l-marker>
            </l-map>
          </div>
      </div>
      <!-- end of Leaflet -->

      <!-- Covid Data -->
      <div class="column is-3 animated zoomIn delay-2s">
      <article class="message is-dark">
        <div class="message-header">
          <div class="field">
          <form @submit.prevent="getApi" class="control">
              <input type="text" v-model="input" placeholder="Enter Country" class="input is-info">
          </form>
          </div>
        </div>
        <div class="message-body">
                <figure class="image is-48x48">
                  <img :src="flag">
                </figure>
              <p> {{ country }} - <i>{{ continent }}</i></p>
              <p>Cases: {{ cases }} </p>
              <p>Deaths: {{ deaths }} </p>
              <p>Recovered: {{ recovered }} </p>
              <p>Cases Today: {{ todayCases }} </p>
              <p>Death Today: {{ todayDeaths }} </p>
        </div>
      </article>
      </div>
      <!-- end of Covid Data -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import {LMap, LTileLayer, LMarker} from 'vue2-leaflet';
import Header from './components/Header'

export default {
  name: 'App',
  components: {
    Header,
    LMap,
    LTileLayer,
    LMarker
  },
  data() {
    return {
      // Covid Api Data
      input: 'philippines',
      cases: '',
      country: '',
      flag: '',
      continent: '',
      deaths: '',
      recovered: '',
      todayCases: '',
      todayDeaths: '',
      // Leaflet Data
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      zoom: 4,
      center: [0, 0],
      bounds: null,
      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      markerLatLng: [0, 0]
    }
  },
  created() {
    this.getApi()
  },
  methods:{
    async getApi() {
      if(this.input == '' || this.input == null) {
        this.input = 'philippines'
      }
      const res = await axios.get(`https://corona.lmao.ninja/v2/countries/${this.input.toLowerCase().trim()}`)
          // console.log(res.data)
          this.country = res.data.country
          this.flag = res.data.countryInfo.flag
          this.cases = res.data.cases
          this.continent = res.data.continent
          this.deaths = res.data.deaths
          this.recovered = res.data.recovered
          this.todayCases = res.data.todayCases
          this.todayDeaths = res.data.todayDeaths
          let lat = res.data.countryInfo.lat
          let long = res.data.countryInfo.long
          this.center = [lat,long]
          this.markerLatLng = [lat, long]

    },
    // Leaflet Method
    zoomUpdated (zoom) {
      this.zoom = zoom;
    },
    centerUpdated (center) {
      this.center = center;
    },
    boundsUpdated (bounds) {
      this.bounds = bounds;
    }
  }
}
</script>

<style scope>
#app {
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
