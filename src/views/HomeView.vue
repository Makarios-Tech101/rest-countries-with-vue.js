<!-- <script setup>
import {ref} from 'vue';
import CountryComponent from '../components/CountryComponent.vue'

const countries = ref([]);


const getCountries =() => {
   fetch('https://restcountries.com/v3.1/all') 
    .then((res) => res.json())
    .then((data) => {
        countries.value = data
    })

    
}

<div class="lds-ring"><div></div><div></div><div></div><div></div></div>

getCountries();

</script> -->

<script setup>
    import { ref, watch } from 'vue';
    import CountryComponent from '../components/CountryComponent.vue';

    const countryData = ref([]);
    const loading = ref(true);

    
    const filteredCountries = ref([]);
    const selectedRegion = ref('');
    const searchQuery = ref('');


    const getCountryData = () => {
    fetch('https://restcountries.com/v3.1/all')
        .then((res) => res.json())
        .then((data) => {
          setTimeout(() => {
        countryData.value = data;
        filteredCountries.value = data;
        loading.value = false; // End loading after delay
      }, 2000); // 2-second delay
    })
        .catch((error) => {
          console.error('Error fetching data:', error);
          loading.value = false; // End loading
        });
    };
    //     countryData.value = data;
    //     console.log(data);
    //     filteredCountries.value = data; 
    //     searchedCountries.value = data;
    //     loading.value = false;
    //     })
    //     .catch((error) => {
    //         console.error('Error fetching data:', error);
    //         loading.value = false;
    //     });
    // };


     // To filter by continent
        const filterCountries = () => {
        loading.value = true; // Start loading
        setTimeout(() => {
          if (!selectedRegion.value) {
            filteredCountries.value = countryData.value;
          } else {
            filteredCountries.value = countryData.value.filter(
              (country) => country.region === selectedRegion.value
            );
          }
          loading.value = false; // End loading after delay
        }, 2000); // 2-second delay
      };


    // Watch the searchQuery and filter countries based on the input
    watch(searchQuery, (newQuery) => {
      loading.value = true; // Start loading
      setTimeout(() => {
        if (newQuery.length >= 3) {
          filteredCountries.value = countryData.value.filter((country) =>
            country.name.common.toLowerCase().includes(newQuery.toLowerCase())
          );
        } else {
          filteredCountries.value = countryData.value;
        }
        loading.value = false; // End loading after delay
      }, 2000); // 2-second delay
    });



 getCountryData();
</script>

<template>
 
  <body class="">
  <!-- <div class="dark"> -->
    <header>
       <div class="heading">
          <div class="headingText">
             <h5>
                Where in the world?
             </h5>
          </div>
          <div class="headingButton">
              <p class="theme">
                <span><svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
                 <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M3 11.507a9.493 9.493 0 0 0 18 4.219c-8.507 0-12.726-4.22-12.726-12.726A9.494 9.494 0 0 0 3 11.507"/>
                </svg></span>
               Dark mode
            </p>
          </div>
       </div>
    </header>
    <main>
       <div class="countryBody">
          <div class="countryFilter">
             
             <!-- SEARCH CONTAINER FOR SEARCHING FOR COUNTRIES  -->
              <div class="searchContainer">
                 <form @submit.prevent>
                    <button type="submit">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16px" height="16px" viewBox="0 0 28 28">
                            <path fill="currentColor" d="M5 11.5a6.5 6.5 0 1 1 13 0a6.5 6.5 0 0 1-13 0M11.5 3a8.5 8.5 0 1 0 5.262 15.176l6.53 6.531a1 1 0 0 0 1.415-1.414l-6.531-6.531A8.5 8.5 0 0 0 11.5 3" />
                        </svg>
                    </button>
                    <input v-model="searchQuery" type="text" id="searchInput" placeholder="Search for a country...">
                 </form>
              </div>
              
              <!-- FILTER CONTAINER FOR FILTERING COUNTRIES BY REGION   -->
              <div class="filterContainer">
                  <select v-model="selectedRegion" @change="filterCountries" class="filterRegion">
                    <option value="" >Filter by Region</option>
                    <option value="Africa">Africa</option>
                    <option value="Americas">Americas</option>
                    <option value="Asia">Asia</option>
                    <option value="Europe">Europe</option>
                    <option value="Oceania">Oceania</option>
                  </select> 
              </div>
          </div>
          <div>
             <!-- <CountryComponent :country="countryData" v-if="!loading" /> -->
             <CountryComponent :country="filteredCountries" v-if="!loading" />
             <!-- <div v-else>Loading...</div> -->
             <div class="loading" v-else><div class="lds-ring"><div></div><div></div><div></div><div></div></div></div>
          </div>
          <!-- <div>
            <p v-for="(item, index) in countries" :key="index">
                {{ item.name.common }}
            </p>
          </div> -->
       </div>
    </main>
  <!-- </div>   -->
  <!-- <CountryComponent/> -->
</body>
</template>



<style scoped>
body {
    padding: 0;
    margin: 0;
    --background-color: white;
    --text-color: black;
    --element-color: white;
    background-color: var(--background-color);
    color: var(--text-color);
    
}

/* .container {
    --background-color: white;
    --text-color: black;
    --element-color: white;
    background-color: var(--background-color);
    color: var(--text-color);
    
} */

.loading{
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
  top: 50%;
 
}

.dark {
    --background-color: hsl(207, 26%, 17%);
    --text-color: white;
    --element-color: hsl(209, 23%, 22%);
}

header {
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.1); 
    background-color: var(--background-color);
}


.lds-ring,
.lds-ring div {
  box-sizing: border-box;
  
}
.lds-ring {
  /* display: inline-block;
  position: relative; */
  
  width: 80px;
  height: 80px;
}
.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid currentColor;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: currentColor transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}



header .heading {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    max-width: 1100px;
    margin:auto;
    height: 60px;
    
}

.headingText h5 {
    font-family: "Nunito Sans", sans-serif;
    font-weight: 700;
    font-style:normal;
    font-size: 16px;
    margin: 0px;
}

.headingButton {
    font-size: 15px;
    cursor: pointer;
    font-family: "Nunito Sans", sans-serif;
}

.headingButton svg {
    align-self: flex-end;
    margin-bottom: -3px;
}

.countryFilter  {
    display: flex;
    height: 60px;
    max-width: 1100px;
    margin: auto;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}

.searchContainer{
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
}

.searchContainer button{
    background-color: var(--element-color );
    color: var(--text-color);
    padding: 10px 12px 10px ;
    margin-right: -5px;
    border: none;
    font-family: "Nunito Sans", sans-serif;
    font-size: 12px;
    /* box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.1); */
}

.searchContainer button svg{
   margin-bottom: -3px;
   /* font-size: 13px; */
}

.searchContainer input::placeholder{
    color: grey;
 }

.searchContainer input[type=text]{
    background-color: var(--element-color );
    color: var(--text-color);
    padding: 10px 70px 10px 5px;
    margin-right: -5px;
    border: none;
    outline: none;
    font-size: 12px;
    border-radius: 0px 5px 5px 0px;
    font-family: "Nunito Sans", sans-serif;
}

.filterContainer select{
    background-color: var(--element-color );
    color: var(--text-color);
    padding: 10px 70px 10px 5px;
    font-family: "Nunito Sans", sans-serif;
    border: none;
    outline: none;
    border-radius:5px;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.1);
}
 /* */
</style>