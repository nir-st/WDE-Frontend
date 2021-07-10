<template>
  <div>
    <div v-if="this.isEmpty" class="err">
      <h1>You have no favorite games yet!</h1>
    </div>
    <div v-if="!this.isEmpty">
      <h1>Your upcoming favorite games:</h1>
      <GamePreview
        v-for="g in games" 
        :id="g.GameId" 
        :hostTeamId ="parseInt(g.HomeTeamId)" 
        :guestTeamId ="parseInt(g.AwayTeamId)" 
        :date="g.Date" 
        :hour="g.Time" 
        :stadium="g.Stadium"
        :referee="g.Referee"
        :key="g.id"
        v-on:favoriteUpdatedEvent="updateGames">
      </GamePreview>
    </div>
  </div>
</template>

<script>
import GamePreview from "./GamePreview.vue";
export default {
  name: "FavoriteGames",
  components: {
    GamePreview
  },
  data() {
    return {
      games: []
    };
  },
  computed: {
    isEmpty() {
      return this.games.length == 0;
    }
  },
  methods: {
    async updateGames(){
      const username = this.$root.store.username;
      try {
        const response = await this.axios.get(
          `http://localhost:3000/users/FavoriteGames/${username}`,
        );
        const games = response.data;
        this.games = [];
        this.games.push(...games);
      } catch (error) {
        console.log("error in update games")
        console.log(error);
      }
    }
  }, 
  mounted(){
    console.log('mounting favorite games..')
    this.updateGames(); 
  }
};
</script>

<style>
body, html {
  color: white;
}
h1 {
  font-size: 28px;
  color: white;
}
</style>
