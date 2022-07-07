<template>
  <div>
    <h2>购物车</h2>
  <button @click="onChange">dianji</button>
  <h1>123132</h1>
  </div>
</template>

<script>
  export default {
    name: "Cart",
    data() {
      return {
        name:"ler"
      }
    },
  
    methods: {
      onChange:()=>{
        console.log(this)
        console.log(this.name)
      }
    },
  }
</script>

<style scoped>

</style>
