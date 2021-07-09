<template>
  <div class="player-preview">
  <b-card
    no-body
    style="max-width: 20rem;"
    v-bind:img-src="imageUrl"
    img-top
  >
    <template #header>
      <h4 class="mb-0">{{fullname}}</h4>
    </template>

    <b-card-body>
      <b-card-title>{{teamname}}</b-card-title>
      <b-card-sub-title class="mb-2">position id: {{position}}</b-card-sub-title>
    </b-card-body>
  </b-card>
  </div>
</template>

<script>
export default {
  props: {
    fullname: String, 
    teamname: String,
    position: Number,
    imageUrl: String
  }
}
</script>

<style>
</style>