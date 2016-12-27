<template>
  <div class="forecast">
    <!-- <table class="table table-striped table-bordered">
      <tr>
        <td v-for="item in items">
          {{ item.dt }}
        </td>
      </tr>
    </table> -->
    <!-- <ul>
      <li v-for="item in items">
        {{ item.weather[0].main}}, from {{ item.temp.min}}&#176;C to {{ item.temp.max}}&#176;C
      </li>
    </ul> -->
    <div v-for="item in items" class="panel panel-primary">
      <!-- , f -->
      <div class="panel-heading">
        <h3 class="panel-title">{{item.myDate}}, {{ item.weather[0].main }}</h3>
      </div>
      <div class="panel-body">
        from {{ item.temp.min}}&#176;C to {{ item.temp.max}}&#176;C
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: [],
      dates: []
    }
  },
  created () {
    for (let i = 0; i < 7; ++i) {
      let now = new Date();
      now.setDate(now.getDate() + i);
      this.dates.push(now.toString().substr(0, 10));
    }

    this.$http.get('http://ipinfo.io').then((response) => {
      response = response.body;
      this.city = response.city;
      this.country = response.region;
      let coordinates = response.loc.split(',');
      let api = 'http://api.openweathermap.org/data/2.5/forecast/daily?';
      let latAndLon = `lat=${coordinates[0]}&lon=${coordinates[1]}`;
      let apiKey = '&appid=45980b72e7ebfaf112af6022f9dcf8d2';
      let units = '&units=metric';

      this.$http.get(api + latAndLon + apiKey + units).then((response) => {
        response = response.body;
        for (let i = 0; i < 7; ++i) {
          this.items.push(response.list[i]);
          this.items[i].myDate = this.dates[i];
        }

        // this.items = response.body.list;
        console.log(this.items)
      });
    });
  },
  mounted () {

  }
}
</script>

<style scoped>
.forecast {
  text-align: left;
  margin-left: 30%;
  margin-right: 30%;
}
</style>