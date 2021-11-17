<template>
  <q-page class="flex flex-center">
    <q-card class="q-ma-sm"
        v-for="item in data_playgroundsList"
        :key="item.id"
    >
      <img :src="item.properties.image.url">

      <q-card-section>
        <div class="text-h6">{{ item.properties.name }}</div>
        <div class="text-subtitle2">{{ item.properties.perex }}</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        {{ item.properties.content }}
      </q-card-section>

      <q-card-section>{{ calcCrow(data_currentLat, data_currentLong, item.geometry.coordinates[1], item.geometry.coordinates[0]).toFixed(1) }} km</q-card-section>
    </q-card>
  </q-page>
  <q-inner-loading
    :showing="data_visible"
    label="Please wait..."
    label-class="text-teal"
    label-style="font-size: 1.1em"
  />
</template>

<script>
import { defineComponent } from 'vue';
import axios from "axios";

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      data_playgroundsList: [],
      data_currentLat: "",
      data_currentLong: "",
      data_visible: true
    }
  },
  methods: {
    getPlaygroundList() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(pos => {
          this.data_currentLat = pos.coords.latitude;
          this.data_currentLong = pos.coords.longitude;
          let currentCoordinates = `${pos.coords.latitude}, ${pos.coords.longitude}`;
          let options = {
            headers: {'x-access-token': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJhZGVnMDA3QGdtYWlsLmNvbSIsImlkIjo4ODUsIm5hbWUiOm51bGwsInN1cm5hbWUiOm51bGwsImlhdCI6MTYyOTcwMDU3NSwiZXhwIjoxMTYyOTcwMDU3NSwiaXNzIjoiZ29sZW1pbyIsImp0aSI6ImVlNTI5ZDZmLTFmMDEtNGY5Mi1hN2U0LTIyY2E4OTAxMGM5NyJ9.ipb8SB59mWX_-HCS5Xh1I6Fmi8rypz7XV2a90uiWyM0'},
            params: {'latlng': currentCoordinates, 'limit': '10'}
          };

          axios.get('https://api.golemio.cz/v2/playgrounds/', options).then(res => {
            this.data_visible = false;
            console.log(res.data.features);
            this.data_playgroundsList = res.data.features;
          });
        });
      }
    },
    calcCrow(lat1, lon1, lat2, lon2) {
      var R = 6371; // Radius of the earth in km
      var dLat = this.deg2rad(lat2-lat1);  // deg2rad below
      var dLon = this.deg2rad(lon2-lon1);
      var a =
        Math.sin(dLat/2) * Math.sin(dLat/2) +
        Math.cos(this.deg2rad(lat1)) * Math.cos(this.deg2rad(lat2)) *
        Math.sin(dLon/2) * Math.sin(dLon/2)
        ;
      var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
      var d = R * c; // Distance in km
      return d;
    },
    deg2rad(deg) {
      return deg * (Math.PI/180)
    }
  },
  mounted() {
    this.getPlaygroundList();
  }
})
</script>
