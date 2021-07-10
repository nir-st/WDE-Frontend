<template>
  <center>
    <div class="outer">
      <h1 v-if="!this.isEmpty">You have no favorite games!</h1>
      <GamePreview v-else v-for="game in games" :key="game.gameId"
        :isPast="false"
        :id="game.GameId"
        :hostTeamId="game.HomeTeamId"
        :guestTeamId="game.AwayTeamId"
        :date="game.Date"
        :hour="game.Time"
        :stadium="game.Stadium"
        :referee="game.Referee"
        v-on:favoriteUpdatedEvent="getData">
      </GamePreview>
    </div>
  </center>
</template>

<script>
import GamePreview from '../components/GamePreview.vue';
export default {
  components: {
      GamePreview
  },
  data() {
    return {
      games: []
    }
  },
  computed :{
      isEmpty: function() {
          return this.games.length > 0
      }
  },
  mounted() {
      this.getData();
  },
  methods: {
    async getData() {
      const username = this.$root.store.username;
      const response = await this.axios.get(
        `http://localhost:3000/users/FavoriteGames/${username}`
      );
      this.games = response.data;
      if (this.games.length < 0) {
          this.isEmpty = false;
      }
      else {
          this.isEmpty = true;
      }
    },
  }
}
</script>

<style scoped>
.outer {
    width: 75%;
    margin-top: 20px;
    padding-top: 20px;
    background-color: rgba(0, 0, 0, 0.6);
    border-radius: 10px;
}

</style>