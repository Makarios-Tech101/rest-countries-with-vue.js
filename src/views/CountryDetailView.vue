<script setup>
import { ref, onMounted, computed } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const loading = ref(true);
const router = useRouter();
const country = ref(null);


  const fetchCountryDetails = (code) => {
    fetch(`https://restcountries.com/v3.1/alpha/${code}`)
      .then((res) => res.json())
      .then((data) => {
        setTimeout(() => {
          country.value = data[0];
          loading.value = false; // End loading after delay
        }, 2000); // 2-second delay
      })
      .catch((error) => {
        console.error('Error fetching data:', error);
        loading.value = false; // End loading
      });
};

// const fetchCountryDetails = async (code) => {
//   const response = await fetch(`https://restcountries.com/v3.1/alpha/${code}`);
//   const data = await response.json();
//   country.value = data[0];
// };

const goBack = () => {
  router.back();
};

const getCountryName = (code) => {
  const foundCountry = countries.value.find((country) => country.cca3 === code);
  return foundCountry ? foundCountry.name.common : code;
};

const countries = ref([]);
const currencyNames = computed(() => country.value?.currencies ? Object.values(country.value.currencies).map(c => c.name).join(', ') : '');
const languageNames = computed(() => country.value?.languages ? Object.values(country.value.languages).join(', ') : '');

onMounted(async () => {
  const code = route.params.code;
  await fetchCountryDetails(code);
  const allCountriesResponse = await fetch('https://restcountries.com/v3.1/all');
  countries.value = await allCountriesResponse.json();
});
</script>

<template>
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
    <div>
      <div class="sub-header">
        <div>
          <button @click="goBack">← Back</button>
          <!-- <h1>{{ item.name.common }}</h1> -->
        </div>
        <div>
          
        </div>
      </div>
        <main v-if="country">
           <div class="country-details-img">
             <img :src="country.flags.svg" :alt="country.name.common + ' flag'" />
           </div>
           <div class="country-details-div">
                <div>
                  <h2>{{ country.name.common }}</h2>
                </div>
                <div class="flexed-details">
                  <div class="first-part">
                      <p><strong>Native Name:</strong> {{ country.name.nativeName?.common || country.name.common }}</p>
                      <p><strong>Population:</strong> {{ country.population }}</p>
                      <p><strong>Region:</strong> {{ country.region }}</p>
                      <p><strong>Sub Region:</strong> {{ country.subregion }}</p>
                      <p><strong>Capital:</strong> {{ country.capital?.[0] }}</p>
                      </div>
                  <div class="second-part">
                      <p><strong>Top Level Domain:</strong> {{ country.tld?.[0] }}</p>
                      <p><strong>Currencies:</strong> {{ currencyNames }}</p>
                      <p><strong>Languages:</strong> {{ languageNames }}</p>
                      <p><strong>Border Countries:</strong></p>
                      <ul>
                        <li v-for="border in country.borders" :key="border">{{ getCountryName(border) }}</li>
                      </ul>
                  </div>
                </div>
            </div>
        </main>
      <div class="loading" v-else><div class="lds-ring"><div></div><div></div><div></div><div></div></div></div>
      <!-- <p v-else>Loading...</p> -->
    </div>
  </template>
<style scoped>
body {
    padding: 0;
    margin: 0;
}

.loading{
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
  top: 50%;
 
}

header {
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.1); 
    background-color: var(--background-color);
    margin:-6px;
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

header {
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.1); 
    background-color: var(--background-color);
    margin:-5px;
    padding: 10px
}

header .heading {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    max-width: 1100px;
    margin:auto;
    height: 40px;
    
}

.headingText h2 {
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

.sub-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    max-width: 1100px;
    margin:auto;
    margin-top: 20px;
    font-family: "Nunito Sans", sans-serif;
  }
  
.sub-header button {
    background: none;
    border: none;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.1); 
    cursor: pointer;
    font-size: 15px;
    padding: 8px 16px;
    font-weight: 700;
    color: black;
    font-family: "Nunito Sans", sans-serif;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    border-radius: 5px;
    transition: transform 0.2s ease-in-out;
  }

  .sub-header button:hover {
    transform: scale(1.05);
    box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.2);
  }
  
  main {
    display: flex;
    flex-direction: row;
    max-width: 1100px;
    column-gap: 100px;
    margin: 80px auto;
    align-items: center;
  }
  

  .country-details-img {
    width: 100%;
  }
  
  img {
    width: 100%;
    /* max-width: 400px; */
  }
  
  
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  ul li {
    display: inline-block;
    margin-right: 10px;
    padding: 8px 16px;
    font-size: 14px;
    box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.2);
    transition: transform 0.2s ease-in-out;
    cursor: pointer;
  }
  
  .country-details-div {
    width: 100%;
  }


  .country-details-div h2{
   margin: 0;
   color: black;
   font-family: "Nunito Sans", sans-serif;
   font-weight: 700;
   font-size: 20px;
  }

  .country-details-div p{
   margin: 0;
   color: black;
   font-family: "Nunito Sans", sans-serif;
   font-weight: 400;
   font-size: 15px;
   line-height: 25px;
  }

  .country-details-div strong{
   font-weight: 700;
  }


  .flexed-details {
    display: flex;
    flex-direction: row;
    column-gap: 0px;
  }

  .first-part p {
   margin-right: 5px;
  }

   /*RESPONSIVENESS */
@media(max-width: 699px ) {

  .country-details-div h2 {
    margin-top: 20px;
  }

  header .heading {
    margin: 0px 20px;
    
   }

  .sub-header {
    margin: 30px 20px;
  } 

  main {
    display: flex;
    flex-direction: column;
    margin: 50px 20px;
  }

  .flexed-details {
    display: flex;
    flex-direction: column;
  
    
  }

  .flexed-details .second-part {
    margin-top: 30px;
    
  }

  .flexed-details p {
    line-height: 28px;
    
  }

  .country-details-img {
    width: 100%;
  }
  
  img {
    width: 100%;
    /* max-width: 400px; */
    margin: auto;
  }

}

  </style>
  