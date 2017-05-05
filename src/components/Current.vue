<template>
  <div class="current">
    <h1 class="location" v-if="geoRes[0]">
      <!-- US filter, try to show 'City, State';
        if no City, show 'State' -->
      <span v-if="geoRes[0].countryCode == 'US'">
        <span v-if="geoRes[0].city">{{ geoRes[0].city }}, {{ geoRes[0].administrativeLevels.level1short }}</span>
        <span v-else>{{ geoRes[0].administrativeLevels.level1long }}</span>
      </span>
      <!-- Non-US filter, try to show 'City, Country';
        if no City, show 'Admin Level 1, Country';
        if no Admin Level 1, show 'Country';
        if no Country, show 'formattedAddress' -->
      <span v-if="geoRes[0].countryCode != 'US'">
        <span v-if="geoRes[0].city">{{ geoRes[0].city }}, </span>
        <span v-else>
          <span v-if="geoRes[0].administrativeLevels.level1long">{{ geoRes[0].administrativeLevels.level1long }}, </span>
        </span>
        <span v-if="geoRes[0].country">{{ geoRes[0].country }}</span>
        <span v-else>{{ geoRes[0].formattedAddress }}</span>
      </span>
      <span class="weak">{{ geoRes[0].zipcode }}</span>
    </h1>
    <div class="row">
      <div class="col main">
        <div>{{ darkRes.currently.time * 1000 | moment("dddd, MMMM Do") }}</div>
        <div>{{ darkRes.currently.summary }}</div>
        <div class="main-temp">
          <div class="icon">
            <span v-if="darkRes.currently.icon === 'clear-day'">
              <WeatherIconClearDay></WeatherIconClearDay>
            </span>
            <span v-if="darkRes.currently.icon === 'clear-night'">
              <WeatherIconClearNight></WeatherIconClearNight>
            </span>
            <span v-if="darkRes.currently.icon === 'cloudy'">
              <WeatherIconCloudy></WeatherIconCloudy>
            </span>
            <span v-if="darkRes.currently.icon === 'partly-cloudy-day'">
              <WeatherIconPartlyCloudyDay></WeatherIconPartlyCloudyDay>
            </span>
            <span v-if="darkRes.currently.icon === 'partly-cloudy-night'">
              <WeatherIconPartlyCloudyNight></WeatherIconPartlyCloudyNight>
            </span>
            <span v-if="darkRes.currently.icon === 'rain'">
              <WeatherIconRain></WeatherIconRain>
            </span>
            <span v-if="darkRes.currently.icon === 'sleet'">
              <WeatherIconSleet></WeatherIconSleet>
            </span>
            <span v-if="darkRes.currently.icon === 'snow'">
              <WeatherIconSnow></WeatherIconSnow>
            </span>
            <span v-if="darkRes.currently.icon === 'wind'">
              <WeatherIconWind ></WeatherIconWind>
            </span>
          </div>
          <div class="temperature">
            <div>{{ Math.round(darkRes.currently.temperature) }}</div>
              <sup :class="units">
                <button class="us" title="Switch to Fahrenheit" @click="unitChangeExtended('us')">°F</button>
                <button class="si" title="Switch to Celsius" @click="unitChangeExtended('si')">°C</button>
            </sup>
          </div>
        </div> <!-- end .main -->
      </div> <!-- end .col -->
      <div class="col details">
        <div>Precipitation: 
          <strong>{{ darkRes.currently.precipProbability }}%</strong>
        </div>
        <div>Cloud Coverage: 
          <strong>{{ darkRes.currently.cloudCover }}%</strong>
        </div>
        <div>Humidity: 
          <strong>{{ darkRes.currently.humidity }}%</strong>
        </div>
        <div>Dew Point: 
          <strong>
            {{ Math.round(darkRes.currently.dewPoint) }}°<span v-if="units === 'us'">F</span><span v-else>C</span>
          </strong>
        </div>
        <div>Wind: 
          <strong>{{ darkRes.currently.windSpeed }} 
            <span v-if="units === 'us'">mph</span><span v-else>kph</span>
          </strong>
        </div>
        <div>Visibility: 
          <strong>{{ darkRes.currently.visibility }} 
            <span v-if="units === 'us'">miles</span>
            <span v-else>km</span>
          </strong>
        </div>
        <div>Sunrise: 
          <strong>{{ darkRes.daily.data[0].sunriseTime * 1000 | moment("h:mm A") }}</strong>
        </div>
        <div>Sunset: 
          <strong>{{ darkRes.daily.data[0].sunsetTime * 1000 | moment("h:mm A") }}</strong>
        </div>
      </div> <!-- end .col -->
    </div> <!-- end .row -->
  </div>
</template>

<script>

import WeatherIconClearDay from '../assets/icons/weather/clear_day.svg'
import WeatherIconClearNight from '../assets/icons/weather/clear_night.svg'
import WeatherIconCloudy from '../assets/icons/weather/cloudy.svg'
import WeatherIconPartlyCloudyDay from '../assets/icons/weather/partly_cloudy_day.svg'
import WeatherIconPartlyCloudyNight from '../assets/icons/weather/partly_cloudy_night.svg'
import WeatherIconRain from '../assets/icons/weather/rain.svg'
import WeatherIconSleet from '../assets/icons/weather/sleet.svg'
import WeatherIconSnow from '../assets/icons/weather/snow.svg'
import WeatherIconWind from '../assets/icons/weather/wind.svg'

export default {
  components: {
    WeatherIconClearDay,
    WeatherIconClearNight,
    WeatherIconCloudy,
    WeatherIconPartlyCloudyDay,
    WeatherIconPartlyCloudyNight,
    WeatherIconRain,
    WeatherIconSleet,
    WeatherIconSnow,
    WeatherIconWind
  },

  props: {
    darkRes: {
      type: Object,
      required: true
    },
    geoRes: {
      type: Array,
      required: true
    },
    units: {
      type: String,
      required: true
    },
    unitChangeExtended: {
      type: Function,
      required: true
    }
  },

  methods: {
    unitChange: function () {
      this.unitChangeExtended()
    }
  }
}
</script>