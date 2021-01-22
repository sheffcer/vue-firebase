<template>
  <div class="container">
    <form class="card" @submit.prevent="createUser">
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
    />
    <!-- <app-users-list></app-users-list> -->
  </div>
</template>

<script>
import AppUserslList from './components/AppUsersList'
import axios from 'axios'
export default {
  data () {
    return {
      name: '',
      users: []
    }
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
      console.log(firebaseData)
      this.users.push({
        firstName: this.firstName,
        id: firebaseData.name
      })
      this.name = ''
    },
    async loadUsers () {
      const url = 'https://vue-firebase-146dd-default-rtdb.europe-west1.firebasedatabase.app/users.json'
      const { data } = await axios.get(url)
      const result = Object.keys(data).map(key => {
        return {
          id: key,
          firstName: data[key].firstName
        }
      })
      this.users = result
      console.log(data)
      console.log(result)
    }
  },
  components: {
    AppUserslList
  }
}
</script>

<style>

</style>
