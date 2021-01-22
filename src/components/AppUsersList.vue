<template>
  <div v-if="users.length !== 0">
    <div class="card" v-for="user in users" :key="user.id">
      <span>{{user.firstName}}</span>
      <button class="btn danger" @click="$emit('remove-user', user.id)">удалить</button>
  </div>
  </div>
  <div class="card" v-else>
    <span>Список пуст</span>
    <button class="btn" @click="$emit('load-data')">Загрузить список</button>
  </div>
</template>

<script>
export default {
  emits: ['load-data', 'remove-user'],
  props: ['users']
}
</script>

<style lang="css" scoped>
  .card {
    display: flex;
    justify-content: space-between;
  }
</style>
