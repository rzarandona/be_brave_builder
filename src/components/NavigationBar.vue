<template>
  <div class="navigation-bar">
    <div class="container">
      <div class="nav-brand">
        Be Brave Customizer
      </div>

      <div class="nav-list">
        <li><a href="https://hp.tractionstaging.com/">Homepage</a></li>
        <li><router-link to="/">Customizer</router-link></li>
      </div>
    </div>
  </div>
</template>

<style scoped>
.navigation-bar {
  background: white;
  padding: 20px;
  box-shadow: 0 10px 15px 0 rgba(0, 0, 0, 0.02);
}
.navigation-bar .container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}

.nav-brand {
  color: #58a241;
  font-weight: bold;
  font-size: 25px;
}
.nav-list {
  display: flex;
  justify-content: flex-end;
}
.nav-list li {
  list-style-type: none;
  margin-left: 20px;
  font-size: 15px;
}
.nav-list li a {
  text-decoration: none;
  color: black;
}
</style>
