<template>
  <div class="form">
    <form @submit.prevent="addInfo">
      <div class="form-input">
        <label for="name">Name</label>
        <input type="text" v-model="name" id="name" />
      </div>
      <div class="form-input">
        <label for="surname">Surname</label>
        <input type="text" id="surname" v-model="surname" />
      </div>
      <div class="form-input">
        <label for="age">Age</label>
        <input type="number" id="age" v-model="age" />
      </div>
      <div class="form-btn">
        <button type="submit">submit</button>
      </div>
    </form>
    <div v-if="showAlert" class="text-alert">Please fill the empty inputs</div>
    <div v-if="infos.length">
      <h1>Saved Persons</h1>
      <ul v-for="info of infos" :key="info.id">
        <li>Name: {{ info.name }}</li>
        <li>Surname: {{ info.surname }}</li>
        <li>Age: {{ info.age }}</li>
        <button @click="deleteInfo(info.id)">Delete Person</button>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
/* --------------------------------- import -------------------------------- */
import { Component, Vue } from 'vue-property-decorator'
import axios from 'axios'

const baseUrl = 'http://localhost:3000/data/'

@Component
export default class SignupForm extends Vue {
  /* ----------------------------------- data( Alt + X) ---------------------------------- */
  private infos: Array<string | number | null> = []
  private name: string = ''
  private surname: string = ''
  private age: number | null = null
  private showAlert: boolean = false

  /* -------------------------------- lifecycle ------------------------------- */
  created() {
    try {
      this.getInfo()
    } catch (e) {
      console.log(e)
    }
  }

  /* --------------------------------- methods -------------------------------- */

  async getInfo() {
    const res = await axios.get(baseUrl)

    this.infos = res.data
  }

  async addInfo() {
    if (this.name === '' || this.surname === '' || this.age === null) {
      this.showAlert = true

      setTimeout(() => {
        this.showAlert = false
      }, 4000)
    } else {
      const res = await axios.post(baseUrl, {
        name: this.name,
        surname: this.surname,
        age: this.age,
      })

      this.infos = [...this.infos, res.data]

      this.name = ''
      this.surname = ''
      this.age = null
    }
  }

  async deleteInfo(id: any) {
    await axios.delete(baseUrl + id)

    const res = await axios.get(baseUrl)

    this.infos = res.data
  }
}
</script>

<style scoped lang="scss">
.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  form {
    padding: 20px;
    margin-top: 30px;
    border: 1px solid #ddd;
    border-radius: 6px;
  }

  &-input {
    display: flex;
    flex-direction: column;
    margin-bottom: 16px;

    label {
      cursor: pointer;
    }
  }

  &-btn {
    display: flex;
    justify-content: center;
  }

  li {
    list-style: none;
  }

  .text-alert {
    margin-top: 14px;
    color: red;
    font-size: 24px;
  }
}
</style>
