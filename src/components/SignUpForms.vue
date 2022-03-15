<template>
  <Alert :btnText="'Terima Kasih ' + name + ' sudah mengisi form ini '" v-if="showModal" closeName="Close" @click="deleteFormsInput" />
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
      <option value="X AGAMA">X AGAMA</option>
      <option value="XI IPA I">XI IPA I</option>
      <option value="XI IPA II">XI IPA II</option>
      <option value="XI IPS I">XI IPS I</option>
      <option value="XI IPS II">XI IPS II</option>
      <option value="XI IPS III">XI IPS III</option>
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
      <label for="skils">E-COMMERCE Yang Dipakai</label>
      <input type="text" name="suka" id="skils" v-model="tempSkills" />
      <label for="pemakaian">Berapa Kali Memakai Ecommers Dalam seminggu</label>
      <input type="text" name="pemakaian" id="pemakaian" v-model="pemakaian" />
      <label for="kegunaan">Apa Yang Biasa Dilakukan Dalam Memakai E-commerce?</label>
      <input type="text" name="kegunaan" id="kegunaan" v-model="kegunaan" />
      <label for="skils" v-show="showMoment">(wait for a moment)</label>
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

    <!-- <div v-for="ecom in skills" :key="ecom" class="pill" v-show="yes">
      <span @click="deleteSkills(ecom)"><input type="hidden" name="suka" id="suka" :value="ecom" />{{ ecom }}</span>
    </div> -->

    <!-- submit -->
    <div class="submitt">
      <button type="submit" :disabled="disable" class="btn" @click="handleSubmit">Submit</button>
    </div>
  </form>
  <Loading v-if="showLoading" />
</template>

<script>
import Alert from './Alert.vue';
import Loading from './Loading.vue';
export default {
  components: {
    Alert,
    Loading,
  },
  data() {
    return {
      name: '',
      email: '',
      role: 'X MIPA I',
      tempSkills: '',
      skills: [],
      alasan2: 'Ya',
      alasan: '',
      kegunaan: '',
      pemakaian: '',
      yes: false,
      showModal: false,
      disable: true,
      required: true,
      options: false,
      showAlasan: true,
      showMoment: false,
      showLoading: false,
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
      form.addEventListener(
        'submit',
        (e) => {
          e.preventDefault();
          fetch(scriptURL, { method: 'POST', body: new FormData(form) })
            .then((response) => console.log('Success!', response))
            .catch((error) => console.error('Error!', error.message));
        },
        ((this.showMoment = true), (this.showLoading = true))
      );
      setTimeout(
        () => (
          (this.showModal = true),
          (this.email = ''),
          (this.skills = []),
          (this.yes = false),
          (this.alasan = ''),
          (this.alasan2 = ''),
          (this.showAlasan = true),
          (this.showMoment = false),
          (this.showLoading = false),
          (this.pemakaian = ''),
          (this.kegunaan = '')
        ),
        5000
      );
    },
    showFeature() {
      this.showAlasan = !this.showAlasan;
    },
    deleteFormsInput() {
      this.name = '';
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
  background: #ff512f; /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #dd2476, #ff512f); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #dd2476, #ff512f); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

  text-align: left;
  padding: 40px;
  border-radius: 10px;
  /* box-shadow: 1px 2px 95px 1px rgba(0, 0, 0, 0.7);
  -webkit-box-shadow: 1px 2px 95px 1px rgba(0, 0, 0, 0.7);
  -moz-box-shadow: 1px 2px 95px 1px rgba(0, 0, 0, 0.7); */
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
  border-radius: 20px;
  background: rgba(0, 0, 0, 0.459);
  border-bottom: 1px solid rgba(0, 0, 0, 0.397);

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
  background-color: rgb(238, 238, 238);
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
  color: rgb(255, 255, 255);
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
