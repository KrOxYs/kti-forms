<template>
  <!-- <ToggleSwitch /> -->
  <div class="opening">
    <h1>Pemanfaat E-commerce untuk mempermudah ativitas siswa man 18 jakarta</h1>
  </div>
  <SignUpForm />
</template>

<script>
import SignUpForm from './components/SignUpForms.vue';
import ToggleSwitch from './components/ToggleSwitch.vue';
export default {
  name: 'App',
  components: {
    SignUpForm,
    ToggleSwitch,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  margin: auto;
  background: rgb(0, 255, 179);
}
.opening {
  max-width: 400px;
  margin: auto;
}
</style>
