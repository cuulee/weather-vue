<template>
  <div id="app">
    <div class="weather-card">
      <Search @setAppState="setAppState(...arguments)" ref="search"/>

      <div class="weather" v-if="store.appState.state === 'loaded'"> 
        <Current @changeUnits="fetchWeather()"/>
        <Forecast/>
      </div>

      <div :class="store.appState.state" class="loading-or-error" v-else>
        <span>{{ store.appState.message }}</span>
        <div class="spinner" v-if="store.appState.state === 'loading'"></div>
      </div>

      <button class="refresh" title="Refresh" @click="fetchWeather()" v-if="store.darkRes.currently">
        <IconRefresh/>
        <span>Last updated: {{ store.darkRes.currently.time * 1000 | moment("h:mm A") }}</span>
      </button> 
    </div>
    <Background/>
  </div>
</template>

<script>
import Background from './components/Background'
import Search from './components/Search'
import Current from './components/Current'
import Forecast from './components/Forecast'
import IconRefresh from './assets/icons/ui/refresh.svg'

export default {
  name: 'app',

  components: {
    Background,
    Search,
    Current,
    Forecast,
    IconRefresh
  },

  computed: {
    store () {
      return this.$store.state
    }
  },

  methods: {
    fetchWeather () {
      this.setAppState('loading')
      this.$refs.search.fetchWeather().then(() => {
        this.setAppState('loaded')
      })
    },
    setAppState (state, message) {
      this.store.appState.state = state
      this.store.appState.message = message
    }
  },

  mounted () {
    if (localStorage.units) {
      this.store.units = localStorage.getItem('units')
    }
  }
}
</script>

<style lang="scss">
@import 'scss/_vars.scss';
@import 'scss/_base.scss';
@import 'scss/partials/_spinner.scss';

#app {
  align-items: center;
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: center;
}

.weather-card {
  background-color: #fbfbfb;
  border-radius: 3px;
  box-shadow: 0px 0px 150px 0px rgba(0,0,0,0.5);
  color: #96969f;
  display: flex;
  flex-direction: column;
  margin-top: 10vh;
  max-width: 800px;
  min-height: 510px;
  padding: 30px;
  position: relative;
  width: 100%;
  z-index: 1;

  > * {
    display: flex;
    flex-direction: column;
  }
}

.weather {
  flex: 1;
  margin-top: 16px;
  padding-bottom: 16px;
  position: relative;

  .current,
  .forecast {
    animation-duration: 0.3s;
    animation-fill-mode: forwards;
    animation-iteration-count: 1;
    animation-name: fadeIn;
    flex: 1;

    @keyframes fadeIn {
      0% {
        opacity: 0;
      }
      100% {
        opacity: 1;
      }
    }

    svg {
      height: auto;
      width: 100%;
    }
  }
}

.loading-or-error {
  align-items: center;
  flex: 1;
  justify-content: center;
  position: relative;

  .spinner {
    margin: -40px auto 0;
  }

  span {
    font-size: 16px;
    height: 16px;
    margin-top: -26px;
    position: absolute;
    top: 50%;
  }

  &.loading span {
    margin-top: 14px;
  }
}

.refresh {
  align-items: center;
  bottom: 10px;
  flex-direction: row;
  left: 10px;
  margin: 0;
  opacity: .75;
  padding: 0;
  position: absolute;

  &:hover {
    svg {
      transform: rotate(45deg);
    }
  }

  svg {
    transition: 0.3s;
  }

  span {
    color: #a1a1a9;
    font-size: 14px;
    margin-left: 5px;
  }
}
</style>
