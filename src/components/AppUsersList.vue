<template>
  <div v-if="users.length !== 0">
    <div class="card" v-for="user in users" :key="user.id">
      {{user.firstName}}
  </div>
  </div>
  <div class="card" v-else>
    <h3>Список пуст</h3>
    <button class="btn" @click="$emit('load-data')">Загрузить список</button>
  </div>
</template>

<script>
export default {
  emits: ['load-data'],
  props: ['users']
}
</script>

<style lang="scss" scoped>

</style>
