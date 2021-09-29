<template>
  <div></div>
</template>

<script>
export default {
  computed: {
    base_url() {
      return process.env.VUE_APP_BASE_URL;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");
body,
body * {
  font-family: Poppins;
}
</style>
