<template>
  <div class="weather">
    <p>You are in&nbsp;{{ city }}, {{ country }}</p>
    <p class="snd-line">{{ temperature }}&#176;<span v-on:click="toggleUnit()">{{ unit }}</span></p>
    <p>from {{ minTemp }}&#176;{{ unit }} to {{ maxTemp }}&#176;{{ unit }}</p>
    <p><span class="label label-warning">{{ des }}</span>&nbsp;{{ weatherDes }}</p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      city: 'some city',
      country: 'some country',
      temperature: 0,
      unit: 'C',
      des: 'To be forcasted',
      showCenti: true,
      maxTemp: 0,
      minTemp: 0,
      weatherDes: '',
    }
  },
  methods: {
    toggleUnit () {
      if (this.showCenti) {
        this.unit = 'C';
        this.temperature = this.cel;
        this.maxTemp = this.celMax;
        this.minTemp = this.celMin;
      } else {
        this.unit = 'F';
        this.temperature = this.fah;
        this.maxTemp = this.fahMax;
        this.minTemp = this.fahMin;
      }
      this.showCenti = !this.showCenti;
    }
  },
  created () {
    this.$http.get('http://ipinfo.io').then((response) => {
      response = response.body;
      this.city = response.city;
      this.country = response.region;
      let coordinates = response.loc.split(',');
      let api = 'http://api.openweathermap.org/data/2.5/weather?';
      let apiKey = '&APPID=45980b72e7ebfaf112af6022f9dcf8d2';
      let latAndLon = `lat=${coordinates[0]}&lon=${coordinates[1]}`;
      let units = '&units=metric';

      this.$http.get(api + latAndLon + units + apiKey).then((response) => {
        response = response.body;
        this.temperature = response.main.temp;
        this.maxTemp = response.main.temp_max;
        this.minTemp = response.main.temp_min;
        this.cel = response.main.temp;
        this.celMin = response.main.temp_min;
        this.celMax = response.main.temp_max;
        this.fah = Math.round(this.cel *  9 / 5 + 32);
        this.fahMin = Math.round(this.celMin *  9 / 5 + 32);
        this.fahMax = Math.round(this.celMax *  9 / 5 + 32);
        this.des = response.weather[0].main;
        this.weatherDes = response.weather[0].description;
      });
    });
  }
}
</script>

<style scoped>
.snd-line {
  font-size: 30pt;
}
.min-max-temp {
  font-size: 14pt;
}
</style>
