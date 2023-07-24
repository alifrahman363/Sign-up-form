<template>
  <form @submit.prevent="handleSubmit" enctype="multipart/form-data">
    <!-- Email -->
    <label>Email:</label>
    <input type="email" required v-model="email">
    <!-- Password -->
    <label>Password:</label>
    <input type="password" required v-model="password">
    <div v-if="passwordError" class="error">{{ passwordError }}</div>
    <!-- Role -->
    <label>Role</label>
    <select v-model="role">
        <option value="designer">Web Designer</option>
        <option value="developer">Web Developer</option>
    </select>
    <!-- skill -->
    <label>Skills:</label>
    <input type="text" v-model="tempSkill" @keyup="addSkill">
    <div v-for="item in skills" :key="item" class="pill">
        <span @click="deleteSkill(item)">{{ item }}</span>
    </div>
    <!-- Uploading CV -->
    <div class="fileUpload">
      <label for="file" class="label">Upload CV</label>
      <span>
        <font-awesome-icon icon="fa-solid fa-user-secret" />
      </span>
      <input type="file" ref="file" @change="handleFileSelect">
    </div>

    <div class="terms">
        <input type="checkbox" required v-model="terms">
        <label>Accept terms and conditions</label>
    </div>

    <div class="submit">
        <button>Create an account</button>
    </div>
    <!-- <div>
        <input type="checkbox" value="experienced" v-model="experience">
        <label>I have work Experience</label>
    </div>
    <div>
        <input type="checkbox" value="inexperienced" v-model="experience">
        <label>I don't have work Experience</label>
    </div> -->
  </form>

  <!-- <p>Email {{ email }}</p>
  <p>Password: {{ password }}</p>
  <p>Role: {{ role }}</p>
  <p>Skills: {{ tempSkill }}</p>
  <p>Terms accepted: {{ terms }}</p> -->
  <!-- <p>Work Experience: {{ experience }}</p> -->

</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      email: '',
      password: '',
      role: '',
      terms: false,
      tempSkill: '',
      skills: [],
      passwordError: '',
      file: ''
    //   experience: []
    }
  },
  methods: {
    addSkill (e) {
      if (e.key === 'Enter' && this.tempSkill) {
        if (!this.skills.includes(this.tempSkill)) {
          this.skills.push(this.tempSkill)
        }
        this.tempSkill = ''
      }
    },
    deleteSkill (skill) {
      this.skills = this.skills.filter((item) => {
        return skill !== item
      })
    },

    handleFileSelect () {
      this.file = this.$refs.file.files[0]
    },

    async handleSubmit () {
      const formData = new FormData()
      formData.append('file', this.file)
      try {
        await axios.post('/upload', formData)
      } catch (err) {
        console.log(err)
      }

      // checking password length
      this.passwordError = this.password.length > 5 ? '' : 'Password should be more than 5 characters'

      // password and term validation
      if (!this.passwordError && this.terms) {
        // consoling the datas
        console.log('Email: ', this.email)
        console.log('Password: ', this.password)
        console.log('Role: ', this.role)
        console.log('Skills: ', this.skills)
        console.log('Terms accepted: ', this.terms)
        console.log('File: ', this.file)
      }
    }
  }
}
</script>

<style>

    form{
        max-width: 420px ;
        margin: 30px auto;
        background: #ccbbdf;
        text-align: left;
        padding: 40px;
        border-radius: 10px;
    }

    label{
        color: #441e7a;
        display: inline-block;
        margin: 25px 0 15px;
        font-size: 0.6em;
        text-transform: uppercase;
        letter-spacing: 1px;
        font-weight: bold;
    }

    input, select{
        display: block;
        padding: 10px 6px;
        width: 100%;
        box-sizing: border-box;
        border: none;
        border-bottom: 1px solid #8c59d5;
        color: #3b1a69;
    }

    input[type='checkbox']{
        display: inline-block;
        width: 16px;
        margin: 0px 10px 0px 0px;
        position: relative;
        top: 2px;
    }

    .pill{
        display: inline-block;
        margin: 20px 10px 0px 0px;
        padding: 6px 12px;
        background: #7450a4;
        border-radius: 20px;
        font-size: 12px;
        letter-spacing: 1px;
        font-weight: bold;
        color: #ccbbdf;
        cursor: pointer;
    }

    button{
        background: #441e7a;
        border: 0px;
        padding: 10px 20px;
        margin-top: 20px;
        color: white;
        border-radius: 20px;
    }

    .submit{
        text-align: center;
    }

    .error{
        color: #990073;
        margin-top: 10px;
        font-size: 0.8em;
        font-weight: bold;
    }
</style>
