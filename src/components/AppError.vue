<template>
  <div
  :v-model="alert"
  v-if="alert"
  :class="alert.type"
  >
  <h3>{{alert.title}}</h3>
  <p>{{alert.text}}</p>
  <button class="btn" :class="alert.type" @click="$emit('close-alert')">Закрыть</button>
  </div>
</template>

<script>
export default {
  emits: ['close-alert'],
  props: ['alert']
}
</script>

<style lang="css" scoped>

</style>
