<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <TestComponent />
  </div>
</template>

<script>
import axios from "axios";
import TestComponent from "./components/TestComponent";

export default {
  name: 'App',
  components: {TestComponent},
  data() {
    return {
      BASE_URL: 'http://localhost:3000/employees',
      savedDataResponse: {},
      model: {
        first_name: '',
        last_name: '',
        email: ''
      }
    }
  },
  created() {
    console.log('APP COMPONENT CREATED')
  },
  mounted() {
    console.log('APP COMPONENT MOUNTED')
    this.getData()
    this.fetchData()
    // This method allows us to search resources with params, for now I've added hardcoded value,
    // but I encourage you to play with it and make it more flexible
    // this.getDataWithProps()
    // this.sendData()
  },
  methods: {
    async getData() {
      console.log(await axios.get(this.BASE_URL));
    },
    async getDataWithProps() {
      const searchParams = new URLSearchParams('first_name=Steve')
      console.log(await axios.get(`${this.BASE_URL}?${searchParams.toString()}`));
    },
    fetchData() {
      fetch(this.BASE_URL).then(resp => {
        return resp.json()
      }).then(data => {
        console.log(data)
      }).catch(error => {
        console.error(error)
      })
    },
    async sendData() {
      try {
        const savedData = await axios.post(this.BASE_URL, this.model)
        this.savedDataResponse = savedData
      } catch (err) {
        console.error(err)
      } finally {
        this.model = {
          first_name: '',
          last_name: '',
          email: ''
        }
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
