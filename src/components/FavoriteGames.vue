<template>
  <div>
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
      :key="g.id"></GamePreview>
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
  methods: {
    async updateGames(){
      const username = this.$root.store.username;
      try {
        const response = await this.axios.get(
          `http://localhost:3000/users/FavoriteGames/${username}`,
        );
        const games = response.data;
        console.log(response)
        this.games = [];
        this.games.push(...games);
      } catch (error) {
        console.log("error in update games")
        console.log(error);
      }
    }
  }, 
  mounted(){
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
