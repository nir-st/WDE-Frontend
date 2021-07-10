<template>
  <div class="div">
    <center>
      <b-card
        no-body
        style="max-width: 20rem;"
        v-bind:img-src="image_url"
        img-alt="Image"
        img-top
      >
      <template #header>
      <h4 class="mb-0">{{ fullname }}</h4>
      </template>

      <b-card-body>
        <b-card-title>{{ teamName }}</b-card-title>
        <b-card-sub-title class="mb-2">Position: {{ positionId }}</b-card-sub-title>
        <b-card-text>
          <b>Common name:</b> {{ commonName }}
        </b-card-text>
      </b-card-body>

      <b-list-group flush>
        <b-list-group-item><b>Birthdate:</b> {{ birthDate }}</b-list-group-item>
        <b-list-group-item><b>Birth country:</b> {{ birthCountry }}</b-list-group-item>
        <b-list-group-item><b>Height:</b> {{ height }}</b-list-group-item>
        <b-list-group-item><b>Weight:</b> {{ weight }}</b-list-group-item>
      </b-list-group>
    </b-card>
    </center>
  </div>
</template>

<script>
export default {
  props: {
    id: Number
  },
  data() {
    return {
      fullname: "",
      teamName: "",
      commonName: "",
      positionId: "",
      image_url: "",
      birthDate: "",
      birthCountry: "",
      height: "",
      weight: ""
    }
  },
  methods: {
    async getData() {
        const response = await this.axios.get(
        `http://localhost:3000/players/player/${this.$route.params.id}`
        );

        console.log(response);

        this.fullname = response.data.fullname;
        this.teamName = response.data.teamName;
        this.commonName = response.data.commonName;
        this.positionId = response.data.positionId;
        this.image_url = response.data.image_url;
        this.birthDate = response.data.birthDate;
        this.birthCountry = response.data.birthCountry;
        this.height = response.data.height;
        this.weight = response.data.weight;

        // this.fullname = 'nir';
        // this.teamName = 'barca';
        // this.commonName = 'nir';
        // this.positionId = 3;
        // this.image_url = 'sdf';
        // this.birthDate = '234';
        // this.birthCountry = '234';
        // this.height = '234';
        // this.weight = '432';
    }
  },
  mounted() {
    this.getData()
  },
}
</script>

<style scoped>
h4 {
    font-size: 28px;
}
.div {
    padding-top: 35px;
    font-size: 18px;
    color: black;
}
</style>