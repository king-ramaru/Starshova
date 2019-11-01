<template> 
      <div  class="container">

          <div class="row" >
              <div class="col-10">
               <div v-for="item in (this.$store.getters.getC)" v-bind:key="item.name">
            <table class="table_col" v-if="bufff.name===item.name">
                    <colgroup>
                        <col>
                    </colgroup>
                <tr>
                    <td><img :src="img_city"></td>
                    <td>{{item.name}}</td>
                    <td>{{item.sys.country}}</td> 
                    <td>[{{item.coord.lat}}°,{{item.coord.lat}}°]</td> 
                </tr>
                <tr>
                    <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_12-128.png"></td>
                    <td>humidity</td>
                    <td>{{item.main.humidity}}</td>
                </tr>
                <tr>
                    <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_43-128.png"></td>
                    <td>temp/AVG</td>
                    <td> {{(item.main.temp-273.75).toFixed(2)}}°С</td>
                    <td> {{item.main.temp_max-item.main.temp_min}}°С</td>
                </tr>
                <tr>
                      <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_2-128.png"></td>
                      <td>main</td>
                     <td> {{item.weather[0].main}}</td>
                </tr> 
                <tr>
                    <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_30-128.png"></td>
                    <td>pressure</td>
                    <td>{{item.main.pressure}} hpa</td>
                </tr>
                 <tr>
                      <td><img :src="img_Weather"></td>
                      <td>description</td>
                      <td> {{item.weather[0].description}}</td>
                </tr>
                <tr>
                      <td><img src=https://cdn2.iconfinder.com/data/icons/thesquid-ink-40-free-flat-icon-pack/68/wind-sock-256.png></td>
                      <td>wind</td>
                      <td> {{item.wind.speed}}</td>
                </tr>
                 <tr>
                      <td><img src=https://cdn1.iconfinder.com/data/icons/hawcons/32/699847-icon-43-wind-256.png></td>
                      <td>deg</td>
                      <td> {{item.wind.deg}}</td>
                </tr>
                     </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
    
    Vue.use(VueAxios, axios)

    export default {
       data: function() {
           return {
               bufff:[],
               img_city:'',
               img_Weather:'',
           };
       },
      mounted: function(){
                Vue.axios.get("http://api.openweathermap.org/data/2.5/weather?q="+this.$route.params.name+"&appid=b8d67052dc6c85fb12d75983c11d464b",)
                .then((resp) =>{this.bufff=resp.data;
                this.img_city="http://openweathermap.org/images/flags/"+(resp.data.sys.country).toLowerCase()+".png";
                this.img_Weather="http://openweathermap.org/img/wn/"+resp.data.weather[0].icon+"@2x.png";
            } );
    },   
    }
</script>
 