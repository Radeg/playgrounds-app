<template>
  <q-page class="flex flex-center">
    <q-card class="my-card"
        v-for="item in playgroundsList"
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
    </q-card>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import axios from "axios";

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      playgroundsList: [],
    }
  },
  methods: {
    getPlaygroundList() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(pos => {
          let currentCoordinates = `${pos.coords.latitude}, ${pos.coords.longitude}`;
          let options = {
            headers: {'x-access-token': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJhZGVnMDA3QGdtYWlsLmNvbSIsImlkIjo4ODUsIm5hbWUiOm51bGwsInN1cm5hbWUiOm51bGwsImlhdCI6MTYyOTcwMDU3NSwiZXhwIjoxMTYyOTcwMDU3NSwiaXNzIjoiZ29sZW1pbyIsImp0aSI6ImVlNTI5ZDZmLTFmMDEtNGY5Mi1hN2U0LTIyY2E4OTAxMGM5NyJ9.ipb8SB59mWX_-HCS5Xh1I6Fmi8rypz7XV2a90uiWyM0'},
            params: {'latlng': currentCoordinates, 'limit': '10'}
          };

          axios.get('https://api.golemio.cz/v2/playgrounds/', options).then(res => {
            this.playgroundsList = res.data.features;
          });
        });
      }
    }
  },
  mounted() {
    this.getPlaygroundList();
  }
})
</script>
