<template>
  <div>
    <div class="top-bar">
      <b-input-group id="search-input">
        <div style="width: 10%;">
          <b-form-select v-model="searchFor" :options="searchOptions"></b-form-select>
        </div>
        <div style="width: 70%;">
          <b-form-input v-model="searchQuery"></b-form-input>
        </div>
        <b-input-group-append>
          <b-button variant="success" v-on:click="search">Search</b-button>
        </b-input-group-append>
      </b-input-group>
        <br/>
    </div>
    <div class=results>
      <div v-show="this.players">
        <div class=player v-for="player in players" :key="player.id">
          <router-link :to="{name:'player', params:{id: player.id} }">
          <PlayerPreview
            :fullname=player.fullname
            :teamname=player.teamName
            :imageUrl=player.image_url
            :position=player.positionId
          ></PlayerPreview>
          </router-link>
        </div>
      </div>
      <div v-show="this.teams">
        <div class=team v-for="team in teams" :key="team.id">
          <TeamPreview
            :name=team.name
            :logoPath=team.logoPath
          ></TeamPreview>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PlayerPreview from '../components/PlayerPreview.vue';
import TeamPreview from '../components/TeamPreview.vue';
export default {
  components: {
    PlayerPreview,
    TeamPreview
  },
  data() {
    return {
      searchFor:'playersOption',
      searchQuery:"",
      searchOptions: [
        { value: 'playersOption', text: 'Players'},
        { value: 'teamsOption', text: 'Teams'}
      ],
      players: [],
      teams: []
    };
  },
  methods: {
    async search() {
      this.teams = [];
      this.players = [];
      if (this.searchFor == "playersOption") {
        const result = await this.axios.get(
          `http://localhost:3000/search/players/${this.searchQuery}`
        );
        this.players = result.data;
      }
      else if (this.searchFor == "teamsOption") {
        const result = await this.axios.get(
          `http://localhost:3000/search/teams/${this.searchQuery}`
        );
        console.log(result.data);
        this.teams = result.data;
      }
    }
  }
}
</script>

<style scoped>

.player {
  margin-left: 30px;
  margin-right: 30px;
  margin-top: 40px;
  width: 260px;
  float: left;
}

.player:hover {
  width: 275px;
}

.team {
  margin-left: 30px;
  margin-right: 30px;
  margin-top: 40px;
  width: 220px;
  float: left;
}

.team:hover {
  width: 250px;
}

.top-bar {
  padding-top: 30px;
  padding-left: 25%;
  padding-right: 25%;
  background-color: rgba(0, 0, 0, 0.6)
}
</style>