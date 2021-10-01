<template>
  <div>
    <NavigationBar />
    <main class="container">
      <div class="step">
        <p>Thanks for waiting! Here's your preview.</p>
        <div
          :style="{ 'background-image': 'url(' + preview_url + ')' }"
          class="preview"
        ></div>
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
.preview {
  height: 543.5px;
  background-size: cover;
  background-position: bottom;
}
a {
  text-decoration: none;
  color: white;
}
</style>
