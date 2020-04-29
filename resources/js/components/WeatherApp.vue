<template>
   <div class="text-white mb-8">
     <div class="places-input text-gray-800">
       <input type="text" class="w-full">
     </div> <!-- places-input END -->
    <div class="weather-container font-sans w-128 max-w-lg overflow-hidden rounded-lg bg-gray-900 shadow-lg mt-4">
      <div class="current-weather flex items-center justify-between px-6 py-8">
        <div class="flex items-center">
          <div>
            <div class="text-6xl font-semibold">{{currentTemperature.actual}}°C</div>
            <div>Feels like {{currentTemperature.feels}}°C</div>
          </div>
          <div class="mx-5"> 
            <div class="font-semibold">{{ currentTemperature.summary }}</div>
            <div>{{ location.city.toUpperCase() }}, {{location.country}}</div>
          </div>
        </div>
        <div><img v-bind:src='currentTemperature.icon' /></div>
      </div> <!-- current-weather END -->

      <div class="future-weather text-sm bg-gray-800 px-6 py-8 overflow-hidden">
        <div class="flex items-center">
            <div class="w-1/6 text-lg text-gray-200 font-semibold">MON</div>
            <div class="w-4/6 px-4 flex items-center">
              <div>icon</div>
              <div class="ml-3">Cloudy with a chance of showers</div>
            </div>
            <div class="w-1/6 text-right">
              <div>5°C</div>
              <div>-2°C</div>
            </div>
        </div>
        <div class="flex items-center mt-8">
            <div class="w-1/6 text-lg text-gray-200 font-semibold">MON</div>
            <div class="w-4/6 px-4 flex items-center">
              <div>icon</div>
              <div class="ml-3">Cloudy with a chance of showers</div>
            </div>
            <div class="w-1/6 text-right">
              <div>5°C</div>
              <div>-2°C</div>
            </div>
        </div>
        <div class="flex items-center mt-8">
            <div class="w-1/6 text-lg text-gray-200 font-semibold">MON</div>
            <div class="w-4/6 px-4 flex items-center">
              <div>icon</div>
              <div class="ml-3">Cloudy with a chance of showers</div>
            </div>
            <div class="w-1/6 text-right">
              <div>5°C</div>
              <div>-2°C</div>
            </div>
        </div>
      </div> <!--future-weather-->



    </div> <!-- weather-container-end -->
   </div>
</template>

<script>
    export default {
        mounted() {
            this.fetchData()
        },
        data(){
          return {
            currentTemperature :{
                actual: '',
                feels: '',
                summary: '',
                icon: ''
            },
            location: {
              city: 'kandy',
              country: ''
            }
          }
        },
        methods: {
          fetchData(){
            fetch(`/api/weather?city=${this.location.city}`)
            .then(response => response.json())
            .then(data => {
              // console.log(data);
              this.currentTemperature.actual = Math.round((data.main.temp/9.5));
              this.currentTemperature.feels = Math.round((data.main.feels_like/9.5));
              this.currentTemperature.summary = (data.weather[0].description).toUpperCase();
              //console.log(`http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`);
              
              this.currentTemperature.icon = `http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
              this.location.country = data.sys.country;
              
            });
          }
        }
    }
</script>

