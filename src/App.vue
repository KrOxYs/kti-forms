<template>
  <!-- <ToggleSwitch /> -->
  <div class="opening">
    <h1>PEMANFAATAN E-COMMERCE UNTUK MEMPERMUDAH AKTIVITAS SISWA MAN 18 JAKARTA</h1>
    <!-- <ToggleSwitch /> -->
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
  color: #ffffff;
  margin-top: 60px;
}
body {
  margin: auto;
  background: #12c2e9; /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #f64f59, #c471ed, #12c2e9); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #f64f59, #c471ed, #12c2e9); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
.opening {
  max-width: 400px;
  margin: auto;
}
.opening h1 {
  text-shadow: 2.5px 2.5px black;
}
</style>
