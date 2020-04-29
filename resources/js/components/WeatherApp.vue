<template>
   <div class="text-white mb-8">
     <div class="places-input text-gray-800">
       <input type="search" id="address" class="form-control w-full" placeholder="Where are we going?">
        <p>Selected: <strong id="address-value">none</strong></p>
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
    </div> <!-- weather-container-end -->
   </div>
</template>

<script>
    export default {
        mounted() {
            this.fetchData()

            var placesAutocomplete = places({
                  appId: 'plDSCUCVMUFO',
                  apiKey: 'a272632ed4c734bff5985af2bd3e64c2',
                  container: document.querySelector('#address'),
                })
                .configure({
                  type: 'city',
                  aroundLatLngViaIP: false,
                });

            var $address = document.querySelector('#address-value')

                placesAutocomplete.on('change', (e) => {
                  //console.log(e.suggestion);
                  
                  $address.textContent = e.suggestion.value
                   this.location.city = `${e.suggestion.name}`
                });

                placesAutocomplete.on('clear', function() {
                  $address.textContent = 'none';
                });      
        },
        watch: {
            location: {
              handler(newValue, oldvalue){
                this.fetchData()
              },
              deep: true
            }
        },
        data(){
          return {
            currentTemperature :{
                actual: '',
                feels: '',
                summary: '',
                icon: ''
            },
            daily:[],
            location: {
              city: 'New York',
              country: ''
            }
          }
        },
        methods: {
          fetchData(){
            fetch(`/api/weather?city=${this.location.city}`)
            .then(response => response.json())
            .then(data => {
              //console.log(data);
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

