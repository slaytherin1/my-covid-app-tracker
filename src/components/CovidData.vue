<template>
<div id="Coviddata">
    <!-- TRACKER -->
    <div class="flex">
        <div class="flex-items">
            <form @submit.prevent="getApi" class="control">
              <input type="text" v-model="input" placeholder="Enter Country" class="input is-info">
            </form>
        </div>
        <div class="flex-items">
            <figure class="image is-48x48">
                <img :src="flag"> 
                <span>{{ country }} 
                    <!-- <i class="fa fa-line-chart" aria-hidden="true"></i> -->
                </span>
                
            </figure>
        </div>
        <div class="flex-items">
            Cases: {{ cases }} 
        </div>
        <div class="flex-items">
            Deaths: {{ deaths }} 
        </div>
        <div class="flex-items">
            Recovered: {{ recovered }}
        </div>
        <div class="flex-items">
            Cases Today: {{ todayCases }}
        </div>
        <div class="flex-items">
            Death Today: {{ todayDeaths }}
        </div>
    </div>
    <!-- END OF TRACKER -->

  <div class="chart">
    <!-- COVID DATA CHART -->
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
    <!-- END OF COVID CHART -->

</div>
</template>

<script>
import axios from 'axios'
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LMarker,
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

    //   Leaflet chart data
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      zoom: 4,
      center: [0, 0],
      bounds: null,
      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      markerLatLng: [0, 0],
      showMap: true
    };
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
          let addComma = new Intl.NumberFormat()
          this.country = res.data.country
          this.flag = res.data.countryInfo.flag
          this.cases = addComma.format(this.cases = res.data.cases)
          this.continent = res.data.continent
          this.deaths = addComma.format(this.deaths = res.data.deaths)
          this.recovered = addComma.format(this.recovered = res.data.recovered)
          this.todayCases = addComma.format(this.todayCases = res.data.todayCases)
          this.todayDeaths = addComma.format(this.todayDeaths = res.data.todayDeaths)
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

<style scoped>
.chart {
    width: 100%;
      /* Full height */
    height: 200vh;
    max-height: 100vh;
  /* Center and scale the image nicely */
    /* background-position: center;
    background-repeat: no-repeat;
    background-size: cover; */
    z-index: -1;
}

.flex {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-content: center;
    padding: 1%;
}

.flex-items {
    font-size: 20px;
    flex-basis: 0px;
    min-height: 10px;
    min-width: 100px;
    margin: 5px;
    padding: 5px;
}

form {
    min-width: 150px;
}
</style>