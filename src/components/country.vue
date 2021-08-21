<template>
  <div class="m-1 p-4 bg-gray-400 rounded-md shadow-lg">
      <div @click="getcountry(country.name)" class="cursor-pointer">{{country.name}}<img :src='country.flag' class="w-20" /></div>
  </div>
</template>

<script>
export default {
    props: ['country', 'getcountry']

}
</script>

<style>

</style>