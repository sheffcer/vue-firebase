<template>
  <div class="container">
      <form class="card" @submit.prevent="createUser">
      <app-error :alert="alert" @close-alert="alert = null"></app-error>
      <h2>Post with FireBase</h2>
      <div class="form-control">
        <label for="name">Веедите имя</label>
        <input type="text" id="name" v-model.trim="name">
      </div>
      <button class="btn primary" type="submit" :disabled="name.length === 0">Отправить</button>
    </form>
    <AppUserslList
    :users="users"
    @load-data="loadUsers"
    @remove-user="removeUser"
    />
    <!-- <app-users-list></app-users-list> -->
  </div>
</template>

<script>
import AppUserslList from './components/AppUsersList'
import AppError from './components/AppError'
import axios from 'axios'
export default {
  data () {
    return {
      name: '',
      users: [],
      alert: null
    }
  },
  mounted () {
    this.loadUsers()
  },
  methods: {
    async createUser () {
      const url = 'https://vue-firebase-146dd-default-rtdb.europe-west1.firebasedatabase.app/users.json'
      const response = await fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          firstName: this.name
        })
      })
      const firebaseData = await response.json()
      this.users.push({
        firstName: this.name,
        id: firebaseData.name
      })
      console.log(this.users)
      this.name = ''
    },
    async loadUsers () {
      const url = 'https://vue-firebase-146dd-default-rtdb.europe-west1.firebasedatabase.app/users.json'
      try {
        const { data } = await axios.get(url)
        if (!data) {
          throw new Error('Список юзеров пуст')
        }
        this.users = Object.keys(data).map(key => {
          return {
            id: key,
            firstName: data[key].firstName
          }
        })
      } catch (error) {
        this.alert = {
          type: 'danger',
          title: 'Ошибка ёпта',
          text: error.message
        }
        console.log(error.message)
      }
    },
    async removeUser (id) {
      await axios.delete(`https://vue-firebase-146dd-default-rtdb.europe-west1.firebasedatabase.app/users/${id}.json`)
      this.users = this.users.filter(user => user.id !== id)
    }
  },
  components: {
    AppUserslList,
    AppError
  }
}
</script>

<style>

</style>
