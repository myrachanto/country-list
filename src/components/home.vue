<template>
<div class="mx-auto w-10/12">
  <div class="py-5 text-black text-2xl font-bold">Country list</div>
  <div class=" flex justify-start items-center"><input type="text" class="m-2 p-2 w-96 focus:rotate-2 focus:outline-none focus:border-b-2 bottom-2" v-model="search" @change="getcountry(search)" placeholder="Search country"/>
  <button @click="getcountry(search)">Submit</button></div>
  <div class="m-2 p-2 bg-gray-500 rounded-xl shadow-xl text-white" v-if="search">
      <!-- <p>{{results}}</p> -->
      <div class="grid grid-cols-2">
          <div class=""><img :src='results[0].flag' class="w-60" /></div>
          <div class="d">
      <p>Name: {{results[0].name}}</p>
      <div class="">
          <p>Located: {{results[0].region}}</p>
          <p> in: {{results[0].subregion}}</p> 
      </div>
      <div class="">
          <p>Capital City: {{results[0].capital}}</p>
          <p>Code: {{results[0].callingCodes[0]}}</p>
          <p>Population: {{results[0].population}}</p>
      </div>
      <input type="number" class="my-2 p-2 ring-2 rounded-lg shadow-lg text-black" :placeholder="results[0].callingCodes[0]" v-model="phone" @keyup="vaildatephone(phone, results[0].callingCodes[0])" @change="vaildatephone(phone, results[0].callingCodes[0])"/>
      <span v-if="num === false" class="m-2 p-2 bg-red-100 text-red-700">This {{phone}} is Invalid </span>
      <span v-else class="m-2 p-2 bg-green-100 text-green-700">This {{phone}} is Valid </span></div></div>
  </div>
  <div class="grid grid-cols-5">
      <country v-for="(country, i) in lists" :key="i" :country="country" :getcountry="getcountry"/>
  </div>
  </div>
</template>

<script>
import axios from 'axios'
import country from './country.vue'
export default {
  components: { country },
    data(){
        return{
            lists: [],
            search: '',
            results: [],
            phone: '',
            num: false
        }
    },
    created(){
        this.getlist()
    },
    methods: {
        async getlist (){
        let countries = await axios.get('https://restcountries.eu/rest/v2/all')
        // console.log(countries)
        this.lists = countries.data
        },
        async getcountry(name){
        this.search = name
        let country = await axios.get(`https://restcountries.eu/rest/v2/name/${this.search}`)
        // console.log(country)
        this.results = country.data
        // console.log('///////////', this.results)
        },
        vaildatephone (phone, code){
            this.num = this.phonecode(phone, code)
            // console.log ('kkkkkkkkkkkk',this.num)
            return
        },
        phonecode(phone, code){
            let t = phone.split('')
            let c = code.split('')
            let count = c.length
            if (t[0] === '+'){

              let v = t.slice(1,count+1)
              let cod = v.join('')
            //   console.log('//////////////', code, phone)
              if (code === cod){
                  let d = this.phonenumber(phone)
                //   console.log('>>>>>>>>', d)
                  if (d === true){
                    return true
                  }
                  return false
              }
              return false
            }
            // console.log('/////////sssssss/', code, phone)
            let v = t.slice(0,count)
            let cod = v.join('')
              if (code === cod){
                  let d = this.phonenumber(phone)
                  if (d=== true){
                    return true
                  }
                  return false
              }
              return false
        },
        phonenumber(phone){
            let t = phone.split('')
            let count = t.length
            if (count > 13){
                return false
            }else if(count < 6){
                return false
            }
            return true
        }
   }

}
</script>

<style scoped>

</style>