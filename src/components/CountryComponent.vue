<!-- <script setup>
import {ref} from 'vue';

const country = ref([]);


const getCountry =() => {
   fetch('https://restcountries.com/v3.1/all') 
    .then((res) => res.json())
    .then((data) => {
        country.value = data
        console.log(data)
    })

    
}

getCountry();
</script> -->


<script setup>
// import { defineProps } from 'vue';

const props = defineProps({
  country: {
    type: Array,
    required: true
  }
});
</script>

<template>
    <div class="countryBody">
        <div class="countryDisplay" id="countryDisplay">
            <a v-for="(item, index) in country" :key="index" href="#" class="countryItem">
                <img :src="item.flags.svg" alt="Country Flag" />
                <div class="countryText">
                    <h5><b>{{ item.name.common }} </b></h5>
                    <p><b>Population: </b>{{ item.population }}</p>
                    <p><b>Region: </b>{{ item.region }}</p>
                    <p><b>Capital: </b>{{ item.capital ? item.capital[0] : 'N/A' }}</p>
                </div>
            </a>
       </div>
    </div>
</template>

<style scoped>
.countryBody {
    padding: 30px 0px;
}

.countryBody .countryDisplay {
    display: flex;
    flex-wrap: wrap;
    grid-template-columns: auto auto auto auto;
    grid-template-rows: auto;
    gap: 30px;
    padding-top: 30px;
    justify-content: center;
    margin: auto;
    max-width: 1100px;
    
}


main .countryBody .countryDisplay a{
    text-decoration: none;
    color:inherit;

}
.countryItem {
    display: flex;
    flex-direction: column;
    width: 250px; 
    object-fit: contain;
    overflow: hidden;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    transition: transform 0.2s ease-in-out;
    /* background-color: var(--element-color );
    color: var(--text-color); */
}

.countryItem:hover{
    transform: scale(1.05);
    box-shadow: 0px 0px 16px 0px rgba(0, 0, 0, 0.2);
}

.countryItem img[data-v-0391d4ea] {
    width: 100%;
    height: auto;
    margin: 0px;
}



.countryText{
    margin-top: -5px;
}


.countryText h5{
    margin: 0px;
    font-family: "Nunito Sans", sans-serif;
    font-size: 20px;
    font-weight: 700;
    line-height: 50px;

}

.countryText p{
    margin: 0px;
    font-family: "Nunito Sans", sans-serif;
    font-size: 16px;
    font-weight: 500;
    line-height: 28px;

}

.countryContainer {
    padding: 50px 0px;
    max-width: 1100px;
    margin:auto;
}

.btnContainer {
    display: flex;
    flex-direction: row;
    justify-content: start;
    align-items: center;
    max-width: 1100px;
    margin:auto;
}

.btnContainer button {
    background-color: var(--element-color);
    color: var(--text-color);
    border: none;
    padding: 8px 26px;
    font-family: "Nunito Sans", sans-serif;
    font-size: 15px;
    font-weight: 600;
    box-shadow: 0px 0px 4px 0px rgba(0, 0, 0, 0.2);
    cursor: pointer;
}

.btnContainer svg {
   margin-bottom: -2px;
}

.btnContainer button:hover {
   transform: scale(1.05);
   transition: transform 0.2s ease-in-out;
}

.countryItem {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: inherit;
}

.countryItem img {
  width: 50px;
  height: auto;
  margin-right: 10px;
}

.countryText {
  display: flex;
  flex-direction: column;
}

.countryText h5 {
  margin: 0;
}

.countryText p {
  margin: 0;
}
</style>
