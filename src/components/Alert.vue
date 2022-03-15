<template>
  <!-- <input type="checkbox" id="toggle" />
  <label for="toggle">Open Overlay</label> -->
  <div v-show="showModal" class="modal">
    <dialog>
      <p>{{ btnText }}</p>

      <p>~KuRo</p>
      <label for="toggle" @click="ClickCLose"> {{ closeName }} </label>
    </dialog>
  </div>
</template>

<script>
import Forms from './SignUpForms.vue';
export default {
  props: ['btnText', 'closeName'],
  components: {
    Forms,
  },
  data() {
    return {
      showModal: true,
    };
  },
  methods: {
    ClickCLose() {
      this.showModal = !this.showModal;
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200&display=swap');

body {
  font-family: monospace;
  font-size: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
  padding: 0;
}
.modal {
  background-color: rgba(0, 0, 0, 0.5);
  width: 500px;
}
label {
  background: skyblue;
  color: white;
  padding: 0.3em 1em;
  border-radius: 4px;
}
@keyframes appear {
  0% {
    opacity: 0;
    transform: translateY(-10px);
  }
}

dialog {
  animation: appear 350ms ease-in 1;
  max-width: 300px;
  display: block;
}
</style>
