<template>
  <div>
    <div class="top-bar">
      <center>
      <b-input-group id="search-input">
        <div style="width: 8%;">
          <b-form-select v-model="searchFor" :options="searchOptions"></b-form-select>
        </div>
        <div style="width: 35%;">
          <b-form-input v-model="searchQuery"></b-form-input>
        </div>
        <b-input-group-append>
          <b-button variant="success" v-on:click="search">Search</b-button>
        </b-input-group-append>
        <b-input-group-append>
          <b-form-select v-show="players.length" class='sortBy' v-model="playersSortBy" :options="playersSortByOptions" @change="sortPlayers"></b-form-select>
        </b-input-group-append>
        <b-input-group-append>
          <b-form-select v-show="players.length" class='sortBy' v-model="filterTeams" :options="filterTeamsOptions" @change="filterPlayers"></b-form-select>
        </b-input-group-append>
        <b-input-group-append>
          <b-form-select v-show="players.length" class='sortBy' v-model="filterPositions" :options="filterPositionsOptions" @change="filterPlayers"></b-form-select>
        </b-input-group-append>
      </b-input-group>
      <br/>
      </center>
    </div>
    <div class=results>
      <div class="alert" v-show="foundResults == false"><center><b>No results found!</b></center></div>
      <div v-show="this.players.length">
        <div class=player v-for="player in filteredPlayers" :key=player.id>
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
      <div v-show="this.teams.length">
        <div class=team v-for="team in teams" :key="team.id">
          <router-link :to="{name:'team', params:{id: team.id} }">
            <TeamPreview
              :name=team.name
              :logoPath=team.logoPath
            ></TeamPreview>
          </router-link>
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
      teams: [],
      playersSortByOptions: [
        { value: 'byName', text: 'Sort by player name'},
        { value: 'byTeamName', text: 'Sort by team name'},
        { value: 'byPositionId', text: 'Sort by player position'}
      ],
      playersSortBy: 'byName',
      foundResults: true,
      filterTeams: 'all',
      filterTeamsOptions: [ { value: 'all', text: 'All Teams' } ],
      filterPositions: 'all',
      filterPositionsOptions: [ { value: 'all', text: 'All Positions' } ],
      filteredPlayers: []
    };
  },
  methods: {
    updateAvailableFilters() {
      this.filterTeamsOptions = [ { value: 'all', text: 'All Teams' } ];
      this.filterPositionsOptions = [ { value: 'all', text: 'All Positions' } ];
      let pos = [];
      let teams = [];
      this.players.forEach((player) => {
        if (!teams.includes(player.teamName)) {
          teams.push(player.teamName);
        }
        if (!pos.includes(player.positionId)) {
          pos.push(player.positionId);
        }
      });
      teams.forEach((team) => {
        this.filterTeamsOptions.push({ value: team, text: team })
      })
      pos.forEach((position) => {
        this.filterPositionsOptions.push({ value: position, text: position })
      })
      this.filterTeams = 'all';
      this.filterPositions = 'all';
    },
    filterPlayers() {
      this.filteredPlayers = this.players.filter((player) => this.filterTeams == 'all' || this.filterTeams == player.teamName)
      .filter((player) => this.filterPositions == 'all' || this.filterPositions == player.positionId)      
    },
    comparePlayers(p1, p2) {
      if (this.playersSortBy == "byName") {
        const name1 = p1.fullname.toUpperCase();
        const name2 = p2.fullname.toUpperCase();
        let comparison = 0;
        if (name1 > name2) {
          comparison = 1;
        } else if (name1 < name2) {
          comparison = -1;
        }
        return comparison;
      }
      else if (this.playersSortBy == "byTeamName") {
        const team1 = p1.teamName.toUpperCase();
        const team2 = p2.teamName.toUpperCase();
        let comparison = 0;
        if (team1 > team2) {
          comparison = 1;
        } else if (team1 < team2) {
          comparison = -1;
        }
        return comparison;
      }
      else if (this.playersSortBy == "byPositionId") {
        const pos1 = p1.positionId;
        const pos2 = p2.positionId;
        let comparison = 0;
        if (pos1 > pos2) {
          comparison = 1;
        } else if (pos1 < pos2) {
          comparison = -1;
        }
        return comparison;
      }
      return 0;
    },
    sortPlayers () {
      console.log(this);
      this.players.sort(this.comparePlayers);
    },
    async search() {
      this.teams = [];
      this.players = [];
      if (this.searchFor == "playersOption") {
        const result = await this.axios.get(
          `http://localhost:3000/search/players/${this.searchQuery}`
        );
        this.players = result.data;
        this.filteredPlayers = this.players;
        if (this.players == "" || this.players.length == 0) {
          this.foundResults = false;
        }
        else { 
          this.foundResults = true;
        }
        this.updateAvailableFilters();
      }
      else if (this.searchFor == "teamsOption") {
        const result = await this.axios.get(
          `http://localhost:3000/search/teams/${this.searchQuery}`
        );
        console.log(result.data);
        this.teams = result.data;
        if (this.team == "" || this.teams.length == 0) {
          this.foundResults = false;
        }
        else { this.foundResults = true; }
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
  background-color: rgba(0, 0, 0, 0.6);
  padding-left: 20%;
}
.sortBy {
  width: 100%;
  margin-left: 50px;
}
.alert {
  background-color: rgba(0, 0, 0, 0.6);
  color: white;
  font-size: 18px;
}
</style>