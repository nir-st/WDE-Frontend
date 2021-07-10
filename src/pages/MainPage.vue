<template>
  <div class="container">
    <center>
      <br/>
      <br/>
      <div class="left">
        <LeagueInfo></LeagueInfo>
      </div>
      <div class="right">
        <LoginPage v-if="!$root.store.username"></LoginPage>
        <FavoriteGames v-else></FavoriteGames>
      </div>
    </center>
  </div>
</template>

<script>
import LeagueInfo from "../components/LeagueInfo";
import FavoriteGames from "../components/FavoriteGames";
import LoginPage from "../pages/LoginPage";
export default {
  components: {
    LeagueInfo, 
    LoginPage, 
    FavoriteGames
  }
};
</script>

<style scoped>
.right {
  width: 50%;
  float: right;
  background-color: rgba(0, 0, 0, 0.6);
  border-radius: 25px;
}
.left {
  width: 50%;
  float: left;
}
</style>
