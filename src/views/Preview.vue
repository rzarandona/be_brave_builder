<template>
  <div>
    <NavigationBar />
    <main class="container">
      <div class="step">
        <p>Thanks for waiting! Here's your preview.</p>
        <img class="preview-image" :src="preview_url" alt="" />
      </div>
      <a target="_blank" :href="pdf_url">
        <button class="submit">DEV : VIEW PDF</button></a
      >
    </main>
  </div>
</template>

<script>
import NavigationBar from "../components/NavigationBar.vue";
export default {
  computed: {
    preview_url() {
      return this.$store.state.preview_url;
    },
    pdf_url() {
      return this.$store.state.pdf_url;
    },
  },
  components: {
    NavigationBar,
  },
};
</script>

<style scoped>
.preview-image {
  width: 100%;
  transition: 0.2s;
}
a {
  text-decoration: none;
  color: white;
}
</style>
