<template>
  <div>
    <h1>Homepage {{base_url}}</h1>
    <h1>Hello, World!</h1>
  </div>
</template>

<script>
export default {
  computed:{
    base_url(){
      return process.env.VUE_APP_BASE_URL
    }
  }
}
</script>