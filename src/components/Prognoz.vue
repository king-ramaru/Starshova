<template> 
    <div  class="container">
        <div class="row">
        <div id="adding" class="col-6">
             <form id="transparent">
                <div id="form-inner">
                    <h3>Добавление города</h3>
        <label>Введите название города: </label>
        <input v-model="inputt"> 
        <button class="btn btn-primary"  @click="add_city()">Добавить</button><br>
                </div>  
          </form>
        </div>
             <div class="col-6">
                <form id="transparent">
                   <div id="form-inner">
                       <h3>Выбор города:</h3>
                           <label>Выберите город: </label>
                           <select id="form-control" v-model="select_city_value" >
                               <option  v-for="item in prognozes" v-bind:key="item.name">{{item}}</option>
                           </select>
                           
                   </div>  
             </form>
              <button class="btn btn-primary"  @click="prognoze_city(select_city_value)">Узнать погоду</button>
           </div>

        </div>
        <div v-for="item in (this.$store.getters.getC)" v-bind:key="item.name">
            <div class="row">
                <table class="table_col" >
                    <colgroup>
                        <col>
                    </colgroup>
                    
                <tr>
                    <td>{{item.name}}</td>
                    <td>{{item.sys.country}}</td> 
                    <td>[{{item.coord.lat}};{{item.coord.lat}}]</td> 
                </tr>
                <tr>
                    <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_12-128.png"></td>
                    <td>humidity</td>
                    <td>{{item.main.humidity}}</td>
                </tr>
                <tr>
                    <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_43-128.png"></td>
                    <td>temp</td>
                    <td> {{item.main.temp}}°С</td>
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
                      <td><img src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_5-128.png"></td>
                      <td>description</td>
                      <td> {{item.weather[0].description}}</td>
                      </tr>
                     </table>
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
        prognozes:[],
        select_city_value:'',
        inputt:'',
           };
       },
      mounted: function(){
          if(this.$store.getters.getC.length==0){
            navigator.geolocation.getCurrentPosition(success=>{
            Vue.axios.get("http://api.openweathermap.org/data/2.5/weather?lat="+success.coords.latitude+"&lon="+success.coords.longitude +"&appid=b8d67052dc6c85fb12d75983c11d464b",)
            .then((resp) =>{ this.$store.commit("increment",resp.data);
            //this.img_city="http://openweathermap.org/images/flags/"+(resp.data.sys.country).toLowerCase()+".png";
            // this.img_Weather="http://openweathermap.org/img/wn/"+this.prognozes.weather[0].icon+"@2x.png";
            this.prognozes.push(resp.data.name);  
            })
            
            })
            
            }
            this.$store.getters.getC.forEach(elem=>{if(!this.prognozes.includes(elem.name))this.prognozes.push(elem.name)})
            
            console.log(this.prognozes)
    },

    methods:{
        add_city:function(){
              console.log(this.prognozes);
            if(!this.prognozes.includes(this.inputt))
            Vue.axios.get("http://api.openweathermap.org/data/2.5/weather?q="+this.inputt+"&appid=b8d67052dc6c85fb12d75983c11d464b",).then((resp) => {
            this.prognozes.push(this.inputt)
            this.$store.commit("increment",resp.data);
            this.inputt='';
        })
        },
        prognoze_city:function(rout){
            this.$router.push("prognozinfo/"+(rout))
        },
    },
       
            
    }
</script>


<style >
            body {
                position: relative;
                padding: 60px 50px;
                background-image: url(https://images.wallpaperscraft.ru/image/gorod_vecher_ulitsa_132504_2560x1440.jpg);
                height: 100vh;
                background-size: cover;
        }       
            body:before {
                content: "";
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
                background: linear-gradient(to right bottom,rgba(43, 44, 78, .5),rgba(104, 22, 96, .5));
                }

          
            #form-inner {position: relative;}
            #form-inner h3 {
                position: relative;
                color: white;
                font-family: sans-serif;
                font-weight: 300;
                font-size: 26px;
                text-transform: uppercase;
                }
            #form-inner h3:after {
                content: "";
                position :absolute;
                left: 0;
                bottom: -6px;
                height: 2px;
                width: 60px;
                background: #1762EE;
                }
            #form-inner label {
                display: block;
                padding-left: 15px;
                font-family:  sans-serif;
                color: rgba(255,255,255,.6);
                text-transform: uppercase;
                font-size: 14px;
                }
            #form-inner input{
                display: block;
                width: 100%;
                padding: 0 15px;
                margin: 10px 0 15px;
                border-width: 0;
                line-height: 40px;
                border-radius: 20px;
                color: white;
                background: rgba(255,255,255,.2);
                font-family:  sans-serif;
                }
            #form-inner select {
                display: block;
                width: 100%;
                padding: 0 15px;
                margin: 10px 0 15px;
                border-width: 0;
                line-height: 40px;
                border-radius: 20px;
                height: 40px;
                color: white;
                background: rgba(255,255,255,.2);
                font-family:  sans-serif;
                }
            #transparent{
                    max-width: 400px;
                }
               button{
                    height: 37px;
                    width: 150px;
                    transition: .4s;
                }
                button:hover{
                    transform: scale(1.03,1.03);
                }
                option{
                    color: black;
                }
                .container{
                    display: flex;
                    flex-direction: column;
                }
                #adding{
                    margin-bottom: 40px;
                }
               .row{
                    display: flex;
                    padding: 50px;
                    width: 70%;
                    margin: 0 auto;
                    justify-content:space-evenly;
                    background-color: rgba(12, 13, 30, 0.68);
                    margin-bottom: 35px;
                }
                @media (max-width: 1114px){  
                  .row{
                        flex-direction: column;
                        justify-content:center;
                    };
                }
                table{
                    z-index: 1;
                }



                .table_col {
                    font-family:monospace;
                    font-size: 14px;
                    width: 660px;
                    
                    text-align: left;
                    border-collapse: collapse;
                    color: rgb(11, 17, 22);
                }
                .table_col td {
                   
                    color: #c8ddf1;
                }
                .table_col td:nth-of-type(2){
                    color: #ffffff;
                    font-size: 17px;
                    font-weight: 500;
                }
                img{
                    width: 30px;
                    height: 30px;
                    padding: 0;
                }

    </style>

























