<script setup>
import { ref, watchEffect, watch } from 'vue'
import axios from 'axios'

const showList = ref(false)

const handleShowList = () =>{
    showList.value = !showList.value
}

const countryData = ref(null)

const getAllcountryData = async() =>{
    const res = await axios.get('https://restcountries.com/v3.1/all')
    console.log(res.data)
    countryData.value = res?.data
}

watchEffect(()=>{
    getAllcountryData()
})

const phone = ref('+880')

const handleSelectCoiuntry = (country) => {
    if(country){
        showList.value = false
        phone.value = `${country?.idd?.root}${country?.idd?.suffixes[0]}`
    }else {
        showList.value = true
    }
}

const searchText = ref(null)

const getCountryBySearchByName = async(name) =>{
    const res = await axios.get(`https://restcountries.com/v3.1/name/${name}`)
    countryData.value = res.data
}

watch(searchText, (newV, oldV) =>{
    console.log('newV =>', newV)
    if(newV){
        getCountryBySearchByName(newV)
    }
})
</script>

<template>
    <section id="phone_number_input">
        <div class="phone_number_input">
            <h1>Phone Number Input</h1>
        </div>
        <div class="phone_number_body">
            <div class="phone_number_input_warpper relative">
                <label class="text-sm font-semibold tracking-wide">Phone Number</label>
                <div class="flex items-center border-2 border-sky-500 rounded-md overflow-hidden w-full">
                    <div @click="handleShowList" class="px-4 py-2 flex items-center gap-3 cursor-pointer bg-gray-300">
                        <p class="text-sm font-semibold tracking-wide">{{ phone }}</p>
                        <div class="flex items-center font-semibold">
                            <svg v-if="!showList" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-3 h-3">
                                <path stroke-linecap="round" stroke-linejoin="round" d="m19.5 8.25-7.5 7.5-7.5-7.5" />
                            </svg>
                            <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-3 h-3">
                                <path stroke-linecap="round" stroke-linejoin="round" d="m4.5 15.75 7.5-7.5 7.5 7.5" />
                            </svg>
                        </div>  
                    </div>
                    <Input type="text" placeholder="Phone number" oninput="this.value = this.value.replace(/[^0-9.]/g, '').replace(/(\..*?)\..*/g, '$1').replace(/^0[^.]/, '0');"/>
                </div>
                <div v-if="showList" class="xl:w-[80%] w-full absolute left-0 bg-white shadow rounded-md">
                    <div class="p-4">
                        <input v-model="searchText" type="text" placeholder="Search" class="border-2 border-sky-500 rounded-md ">
                        <div class="w-full bg-slate-100 py-[1px] my-4"></div>
                        <div class="min-h-auto max-h-[300px] overflow-y-auto">
                            <div @click="handleSelectCoiuntry(country)" v-for="country in countryData" class="flex items-center gap-2 cursor-pointer mb-3 last:mb-0 transition-all duration-150 ease-in-out hover:bg-slate-100 p-2">
                                <img class="w-[40px] h-[25px] aspect-video" :src="country?.flags?.png" alt="">
                                <div v-if="country?.idd?.suffixes" class="text-sm font-semibold tracking-wide leading-3 w-full">{{ country?.idd?.root }}{{ country?.idd?.suffixes[0] }} ({{ country?.cca2 }} - {{ country?.name?.common }})</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
#phone_number_input {
    @apply md:px-0 px-4 mb-[500px];
}
.phone_number_input {
    @apply text-center text-lg tracking-wide font-semibold mt-4;
}
.phone_number_body {
    @apply bg-slate-100 rounded-md px-6 py-8 xl:w-1/3 md:w-1/2 w-full mx-auto mt-4 shadow;
}
.phone_number_input_warpper {
    
}
.phone_number_input_warpper input {
    @apply w-full px-4 py-2 text-sm outline-none bg-transparent;
}
</style>