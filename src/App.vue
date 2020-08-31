<template>
  <div id="app" >
    <div class="dropdown">
      <h3>City:</h3>
      <select v-model="selectedCity">
        <option value="">Select a city</option>
        <option v-for="el in datalist" :value="el.city">{{el.city}}</option>
      </select>
    </div>
    <div class="dropdown">
      <h3>Airport:</h3>
      <select  :disabled="selectedCity.length == 0">
        <option value="">Select an airport</option>
        <option v-for="a in airportsList">{{a.name + ", (" + a.iata + "), " + a.passengers}}</option>
      </select>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data: function(){
    return {
      datalist: [],
      airportsList: [],
      index: Number,
      selectedCity:"",
    }
  },
  mounted() {
    // get data from json file
    fetch('https://my-json-server.typicode.com/alenashch/demodb/profile')
        .then(response => response.json())
        .then(json => {
          this.datalist = json
        })
  },
  watch: {
    /*
       get airports for the selected city
       output: airportsList containing a list of airports for the selectedCity
    */
    selectedCity: function() {
      this.airportsList = []
      if (this.selectedCity.length > 0) {
        for (let i = 0; i < this.datalist.length; i++){
          if(this.datalist[i].city==this.selectedCity)
            this.index=i
        }

        for(let i = 0; i < this.datalist[this.index].airports.length; i++){
          this.airportsList[i] = this.datalist[this.index].airports[i]
        }
      }
    }
  },
  components: {

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

.dropdown {
  margin: 10px 0;
  padding: 10px 0;
  border-bottom: 1px solid #DDD;
}

.dropdown span {
  display:inline-block;
  width: 200px;
}
</style>
