<template>
  <div class="form">
    <h3>Check the weather now:</h3>
    <CustomInput />

    <input class="input" type="search" placeholder="Type city name...">
    <button class="btn" @click="searchHandler">
      Search
    </button>
    <ul>
      <li v-for="oneCity in myOptions" :key="oneCity.id">
        {{ oneCity.name }}
      </li>
    </ul>

    <input
      v-model="city"
      class="input"
      type="text"
      placeholder="Input city name"
    >

    <button class="btn" @click="getWeather">
      Get weather
    </button>

    <div :class="outClasses.join(' ')">
      <div>{{ temperature }}</div>
      <div>{{ wind }}</div>
      <div>{{ pressure }}</div>
    </div>

    <div :class="out404Classes.join(' ')">
      {{ notFound }}
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import CustomInput from '../components/CustomInput.vue'
import { cities } from '~/data/cities'

export default {
  components: { CustomInput },
  data () {
    return {
      city: '',
      temperature: '',
      wind: '',
      pressure: '',
      notFound: '',
      outClasses: ['output'],
      out404Classes: ['out404'],
      myValue: '',
      myOptions: []
    }
  },

  computed: {
    searchHandler () {
      return this.myOptions
    }
  },

  created () {
    this.myOptions = cities
  },

  methods: {
    async getWeather () {
      if (!this.city) {
        alert('Input city name')
        return
      }

      const { data } = await axios.get(
        `http://127.0.0.1:1880/weather/${this.city}`
      )
      // console.log(data);
      // this.city = '';

      if (!data.content.main) {
        this.notFound = data.content
        this.outClasses = ['output']
        this.out404Classes.push('show')
      } else {
        this.out404Classes = ['out404']
        this.outClasses.push('show')
        this.temperature = `Temperature: ${Math.round(
          data.content.main.temp
        )}°C`
        this.wind = `Wind: ${Math.round(data.content.wind.speed)}m/s`
        this.pressure = `Pressure: ${Math.round(
          data.content.main.pressure * 0.001
        )}bar`
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.form {
  padding: 20px;
  margin: 20px 0px 30px 0px;
  display: flex;
  flex-direction: column;
}

.select-city {
  width: 100%;
}

.input {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin: 10px 0px 0px 0px;
}

.btn {
  align-self: center;
  margin: 10px 0px 0px 0px;
  padding: 10px 50px;
  background: none;
  color: teal;
  border: 1px solid teal;
}

.output,
.out404 {
  margin: 10px 0px 0px 0px;
  padding: 20px;
  display: none;
  background-color: rgb(240, 240, 240);
  box-shadow: 4px 3px 15px 0px #000000;
  border-radius: 5px;
}

.out404 {
  text-transform: uppercase;
  color: rgb(223, 121, 121);
}

.show {
  display: block;
}
</style>
