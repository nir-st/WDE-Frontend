<template>
  <div class="player-preview">
  <b-card
    no-body
    style="max-width: 20rem;"
    v-bind:img-src="logoPath"
    img-top
  >
    <template #header>
      <h4 class="mb-0">{{name}}</h4>
    </template>
  </b-card>
  </div>
</template>

<script>
export default {

  props: {
    name: String, 
    logoPath: String,
  }
}
</script>

<style>
</style>