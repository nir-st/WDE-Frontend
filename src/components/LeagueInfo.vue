<template>
  <div class="league-preview">
      <b-card
      img-alt="Image"
      tag="article"
      style="max-width: 20rem;"
      class="mb-2"
      bg-variant="dark"
      text-variant="white"
    >
      <b-card-title>{{leagueName}}</b-card-title>
      <b-card-text>
        Season: {{ season }}
        <br/>
        Stage: {{ stage }}
      </b-card-text>
    </b-card>
  </div>
</template>

<script>
export default {
 data() {
    return {
      leagueName: "superliga", 
      season: "season", 
      stage: "stage"
    };
  },
  methods: {
    async getData() {
      try {
        const response = await this.axios.get(
          `http://localhost:3000/league/getDetails`
        );
      this.leagueName = response.data.league_name;
      this.season = response.data.current_season_name;
      this.stage = response.data.current_stage_name;
      } catch(err) {
        console.log(err);
      }
    }
  },
  mounted() {
    this.getData();
  }
}
</script>

<style>
.league-preview {
  display: inline-block;
  width: 250px;
  height: 200px;
  position: relative;
  margin: 10px 10px;
  border-style: solid;
  border-radius: 10px;
  border-width: 5px;
  border-color:rgb(44, 89, 116);
  background-color: rgba(0, 0, 0, 0.5);
}

.b-card {
  background-color: rgba(0, 0, 0, 0.5);
}

.league-preview .league-title {
  text-align: center;
  text-transform: uppercase;
  color:  rgb(111, 155, 197);
}

.league-preview .league-content {
  width: 100%;
  overflow: hidden;
}

</style>