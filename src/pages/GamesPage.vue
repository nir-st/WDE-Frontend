<template>
  <center>
    <div class="outer">
      <b-card no-body>
        <b-tabs v-model="tabIndex" card align="center">
          <b-tab title="Future Games" :title-link-class="linkClass(0)">
            <GamePreview v-for="game in futureGames" :key="game.gameId"
              :isPast="false"
              :id="game.gameId"
              :hostTeamId="game.homeTeamId"
              :guestTeamId="game.awayTeamId"
              :date="game.date"
              :hour="game.time"
              :stadium="game.stadium"
              :referee="game.referee">
            </GamePreview>
          </b-tab>
          <b-tab title="Past Games" :title-link-class="linkClass(1)">
            <GamePreview v-for="game in pastGames" :key="game.gameId"
              :isPast="true"
              :id="game.gameId"
              :hostTeamId="game.homeTeamId"
              :guestTeamId="game.awayTeamId"
              :date="game.date"
              :hour="game.time"
              :stadium="game.stadium"
              :referee="game.referee"
              :homeTeamScore="game.homeTeamScore"
              :awayTeamScore="game.awayTeamScore"
              :eventLog="game.eventLog">
            </GamePreview>
          </b-tab>
        </b-tabs>
      </b-card>
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
      tabIndex: 0,
      futureGames: [],
      pastGames: [],
      isFuture: false,
      isPast: false
    }
  },
  mounted() {
      this.getData();
  },
  methods: {
    async getData() {
      const future = await this.axios.get(
          `http://localhost:3000/games/allFutureGames`
      );
      const past = await this.axios.get(
          `http://localhost:3000/games/allPastGames`
      );
      if (future.data && future.data.length > 0) {
        this.futureGames = future.data;
        this.isFuture = true;
      }
      else { this.isFuture = false; }
      if (past.data && past.data.length > 0) {
        this.pastGames = past.data;
        this.isPast = true;
      }
      else { this.isPast = false; }
    },
    linkClass(idx) {
      if (this.tabIndex === idx) {
        return ['bg-primary', 'text-light']
      } else {
        return ['bg-light', 'text-info']
      }
    }
  }
}
</script>

<style scoped>
.outer {
    width: 80%;
    margin-top: 20px;
    padding-top: 20px;
}
</style>