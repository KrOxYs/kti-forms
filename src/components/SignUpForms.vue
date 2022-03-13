<template>
  <Alert btnText="Terima Kasih Sudah Mengisi Form Ini ;)" closeName="Close" v-if="showModal" />
  <form name="submit-to-google-sheet">
    <label for="name">Nama : </label>
    <!-- menggunakan v-model untuk mengsingkronisasikan dengan property yang di inginkan  -->
    <input type="name" name="name" id="name" v-model="name" required />

    <label for="email">Email : </label>
    <!-- menggunakan v-model untuk mengsingkronisasikan dengan property yang di inginkan  -->
    <input type="email" name="email" id="email" v-model="email" required />

    <!-- pass -->
    <!-- <label for="password">password : </label>
    <input type="password" name="password" id="password" v-model="password" required />
    <div class="error" v-if="passwordError">{{ passwordError }}</div> -->

    <!-- role -->
    <label for="role">Kelas : </label>
    <select name="role" id="role" v-model="role" required>
      <option value="X MIPA I">X MIPA I</option>
      <option value="X MIPA II">X MIPA II</option>
      <option value="X IPS I">X IPS I</option>
      <option value="X IPS II">X IPS II</option>
      <option value="X IPS III">X IPS III</option>
      <option value="XI IPA I">XI IPA I</option>
      <option value="XI IPA II">XI IPA II</option>
      <option value="XII IPA I">XII IPA I</option>
      <option value="XII IPA II">XII IPA II</option>
      <option value="XII IPS I">XII IPS I</option>
      <option value="XII IPS II">XII IPS II</option>
    </select>

    <!-- yes -->
    <div class="yes">
      <input type="checkbox" name="ecommers" id="yes" v-model="yes" @click="showFeature" />
      <label for="yes">Sudah Pernah Berbelanja di E-COMMERCE </label>
    </div>
    <div v-show="yes">
      <!-- skills -->
      <label for="skils">E-COMMERCE Kesukaan (</label>
      <label for="skils"
        ><strong><p>ditambah (.) diakhir nya)</p></strong></label
      >
      <input type="text" name="kesukaan" id="skils" v-model="tempSkills" @keyup="addSkills" />
    </div>
    <div v-show="showAlasan">
      <label for="alasan">Apa Alasan Anda Belum Berbelanja Di E-commerce</label>
      <input type="text" name="alasan" id="alasan" v-model="alasan" />

      <label for="alasan2">Apakah Anda Akan Menggunakan E-commerce?</label>
      <select name="alasan2" id="alasan2" v-model="alasan2">
        <option value="Ya">Ya</option>
        <option value="Tidak">Tidak</option>
      </select>
    </div>

    <div v-for="ecom in skills" :key="ecom" class="pill" v-show="yes">
      <span @click="deleteSkills(ecom)"><input type="hidden" name="suka" id="suka" :value="ecom" />{{ ecom }}</span>
    </div>

    <!-- submit -->
    <div class="submitt">
      <button type="submit" :disabled="disable" class="btn" @click="handleSubmit">Submit</button>
    </div>
  </form>
</template>

<script>
import Alert from './Alert.vue';

export default {
  components: {
    Alert,
  },
  data() {
    return {
      name: '',
      email: '',
      role: 'X MIPA I',
      yes: false,
      tempSkills: '',
      skills: [],
      showModal: false,
      disable: true,
      required: true,
      options: false,
      showAlasan: true,
      alasan2: 'Ya',
      alasan: '',
    };
  },
  methods: {
    addSkills(e) {
      //   cek jika user mengetik tanda koma(,)
      // dan cek jika user mengetik dan menekan alt + ,
      if (e.key === '.' && this.tempSkills) {
        // apa yang diketikan tidak sama dengan yang didalam skills
        if (!this.skills.includes(this.tempSkills)) {
          this.skills.push(this.tempSkills);
        }
        // set ke default
        this.tempSkills = ' ';
      }
    },
    deleteSkills(skill) {
      // mdembuat value baru dari property skills
      // menyaring array
      // jika mereturn true tidak akan menghapus array
      // jika mereturn false akan menghapus array
      this.skills = this.skills.filter((item) => {
        return skill !== item;
      });
    },
    handleSubmit() {
      const scriptURL = 'https://script.google.com/macros/s/AKfycbxhJLx1X-CTLc2hn1sD6Q9Sru_mz4z57LIkSWduEM2HZ9ndSJ0wNEKbs6YMVDIlDYmOGg/exec';
      const form = document.forms['submit-to-google-sheet'];

      form.addEventListener('submit', (e) => {
        e.preventDefault();
        fetch(scriptURL, { method: 'POST', body: new FormData(form) })
          .then((response) => console.log('Success!', response))
          .catch((error) => console.error('Error!', error.message));
      });
      setTimeout(() => ((this.showModal = true), (this.name = ''), (this.email = ''), (this.skills = []), (this.yes = false), (this.alasan = ''), (this.alasan2 = '')), 4000);
    },
    showFeature() {
      this.showAlasan = false;
    },
  },
  updated() {
    if (this.name !== '' && this.email !== '' && this.yes !== false) {
      this.disable = false;
    } else if (this.name !== '' && this.email !== '' && this.alasan !== '' && this.alasan2) {
      this.disable = false;
    } else {
      this.disable = true;
    }

    if (this.yes === true) {
      this.alasan2 = '';
    }
  },
};
</script>

<style>
form {
  max-width: 360px;
  margin: 30px auto;
  background-color: rgb(74, 42, 219);
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
.btn:disabled {
  background: #d0e2fd;
}
label {
  color: rgb(255, 255, 255);
  display: inline-block;
  margin: 25px 0px 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input,
select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  display: block;
  box-sizing: border-box;
  border: none;
  border-radius: 10px;
  background: rgb(74, 42, 219);
  border-bottom: 1px solid rgba(29, 28, 28, 0.877);

  color: rgb(255, 255, 255);
}
input[type='checkbox'] {
  display: inline-block;
  width: 16px;
  margin: 0px 10px 0px 0px;
  position: relative;
  top: 2px;
}
.pill {
  display: inline-block;
  margin: 20px 10px 0 0;
  padding: 6px 12px;
  background-color: #eee;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
  cursor: pointer;
}
button {
  background: #0b6dff;
  border: none;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
}
.submitt {
  text-align: center;
}
.error {
  color: #ff0062;
  margin-top: 10px;
  font-size: 0.8rem;
  font-weight: bold;
}
</style>
