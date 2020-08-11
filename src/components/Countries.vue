<template>
    <div class="container row">

        <!-- Nav -->
    <div class="nav">
      <img src="../assets/logo.png" class="logo"/>
      </div>

      <br><br>
      <h1 class="title">{{ title }}</h1>
      <br><br>

        <div class="column country-info">
            <form>
                   <!-- Countries -->
                    <div class="form-controls">
                        <label for="countries" class="select-label">Select Country</label>
                        <select name="countries" id="countries" v-model="selectedCountry" @change="onCountrySelect($event)" class="count-select">
                            <option :value="country.name" v-for="country in countries" :key="country.id" class="count-option">{{ country.name }}</option>
                        </select>
                    </div>
                    <br><br>

                    <!-- States -->
                    <div class="form-controls">
                        <label for="state" class="select-label">Select State</label>
                        <select name="states" id="states" v-model="selectedState" @change="onStateSelect($event)" class="count-select">
                            <option :value="state.name" v-for="state in states" :key="state.id" class="count-option">{{ state.name}}</option>
                        </select>
                        <div v-if="stateError" :class="{empty: stateError}">
                            <p>This Country Has No States!</p>
                        </div>
                    </div>
                    <br><br>

                    <!-- Cities -->
                    <div class="form-controls">
                        <label for="cities" class="select-label">Select City</label>
                        <select name="cities" id="cities" v-model="selectedCity" class="count-select">
                            <option :value="city.name" v-for="city in cities" :key="city.id" class="count-option">{{ city.name}}</option>
                        </select>
                        <div v-if="cityError" :class="{empty: cityError}">
                            <p>This State Has No Cities!</p>
                        </div>
                    </div>      
            </form>
        </div>

        <!-- Selected -->
        <div class="column-2">
            <div class="selected-options">
            <p v-if="selectedCountry"><span class="s-o">Country:</span> {{ selectedCountry }}</p>
            <br>

            <p v-if="selectedState"><span class="s-o">State:</span> {{ selectedState }}</p>
            <br>

            <p v-if="selectedCity"><span class="s-o">City:</span> {{ selectedCity }}</p>
            </div>

        </div>
    </div>
</template>

<script>
export default {
  name: 'countries',
  data() {
     return {
            title: 'WeJapa Countries',
            selectedCountry: "",
            selectedState: "",
            selectedCity: "",
            stateError: false,
            cityError: false,
            countries: [],
            states: [],
            cities: []
        }
    },
    created() {
        this.$http.get('https://raw.githubusercontent.com/dr5hn/countries-states-cities-database/master/countries%2Bstates%2Bcities.json').then(
            response=> {
                return response.json()
            }
        ).then(data=> {
            this.countries = data.slice(0, 251);
        })    
    },
    methods: {
        onCountrySelect(event) {
            const state = event.target.value
            const index = this.countries.findIndex((x=>x.name === state))
            const countryState = this.countries[index]['states']
            if (countryState.length === 0) {
                this.stateError = !this.stateError 
                this.states = null   
                this.cities = ""
            } else {
                this.states = countryState
                this.stateError = false
            }
        },
        onStateSelect(event) {
            const state = event.target.value
            const index = this.states.findIndex((x=>x.name === state))
            const city = this.states[index]['cities']
            if (city.length === 0) {
                this.cityError = !this.cityError
            } else {
                this.cities = city
                this.cityError = false
            }

        }
    }
}
</script>

<!-- CSS -->
<style scoped>
@import url('https://cdn.rawgit.com/mfd/09b70eb47474836f25a21660282ce0fd/raw/e06a670afcb2b861ed2ac4a1ef752d062ef6b46b/Gilroy.css');

.nav {
  position: relative;
  width: 100%;
  height: 4em;
  background: #000;
  margin: 0 !important;
  padding: 0 !important;
}

.logo {
  position: relative;
  width: 42px !important;
  height: auto !important;
  top: 50%;
  left: 5%;
  transform: translate(-5%, -50%);
}

.title {
  text-align: center;
}

.country-info {
    padding: 20px;
    font-family: 'Gilroy', sans-serif !important;
}

.count-select  {
  width: 20em;
  height: 2.4em;
  display: block;
  margin-top: 15px;
  padding-left: 1em;
  border: 1px solid #e2e8f0;
  line-height: 1.5em;
  outline: none;
  border-radius: 6px;
  font-family: 'Gilroy', sans-serif !important;
  font-size: 16px !important;
  background: url('../assets/down.svg') no-repeat right;
  background-size: 22px 22px;
  cursor: pointer;
  -webkit-appearance: none;
  background-position-x: 17.8em;
}

.count-option {
    display: block;
    margin-left: 20px;
    font-family: 'Gilroy', sans-serif !important;
}

.column {
    float: left;
    width: 50%;
    padding-left: 20% !important;
    height: 100vh;
}

.column-2 {
    float: left;
    width: 50%;
    padding-left: 5% !important;
    height: 100vh;
}

.row:after {
  content: "";
  display: table;
  clear: both;
}

.empty {
    border: 1px solid;
    background: rgb(226, 64, 64);
    color: #fff;
    width: 20em;
    height: 3em;
    padding-left: 1em !important;
    border-radius: 6px;
    margin-top: 20px;
}

.empty p {
    position: relative;
    top: 50% !important;
    transform: translateY(-50%) !important;
}

.selected-options {
    margin-top: 10px;
}

.select-label, .s-o {
  font-size: 18px;
  font-weight: bold;  
}

/* Media Queries */
/* 1024px */
@media screen and (max-width: 1024px) {
  .column {
    float: left;
    width: 50%;
    padding-left: 12% !important;
    height: 100vh;
}

.column-2 {
    float: left;
    width: 50%;
    padding-left: 10% !important;
    height: 100vh;
}
}

/* 768px */
@media screen and (max-width: 768px) {
  .column {
    float: left;
    width: 50%;
    padding-left: 8% !important;
    height: 100vh;
}

.column-2 {
    float: left;
    width: 50%;
    padding-left: 10% !important;
    height: 100vh;
}
}

/* 575px */
@media screen and (max-width: 575px) {
  .column {
    float: left;
    width: 50%;
    padding-left: 8% !important;
    margin-bottom: 2em !important;
    height: auto;
}

.column-2 {
    position: relative;
    margin-bottom: 2em;
    padding-left: 8% !important;
    left: 50%;
    transform: translateX(-50%) !important;
    width: 100% !important;
    height: auto;
}
}

/* 414px */
@media screen and (max-width: 414px) {
  .column, .column-2 {
    padding-left: 12% !important;
}
}

/* 375px */
@media screen and (max-width: 375px) {
  .column, .column-2 {
    padding-left: 8% !important;
}
}

/* 360px */
@media screen and (max-width: 360px) {
  .column, .column-2 {
    padding-left: 6% !important;
}
}

/* 320px */
@media screen and (max-width: 320px) {
  .title {
    font-size: 28px !important;
  }

  .column, .column-2 {
    padding-left: 8% !important;
}

.count-select {
  width: 17em !important;
  background-position-x: 14.8em;
}

.empty {
  width: 17em !important;
}
}
</style>

