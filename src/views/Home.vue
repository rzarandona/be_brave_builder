<template>
  <div>
    <h1>We are traction</h1>
  </div>
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
