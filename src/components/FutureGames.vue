<template>
<div>

<table class="table table-dark">
  <thead>
    <tr>
      <th scope="col">Game-ID</th>
      <th scope="col">Host-Team</th>
      <th scope="col">Away-Team</th>
      <th scope="col">Date</th>
      <th scope="col">Time</th>
      <th scope="col">Stadium</th>
      <th scope="col">Referee</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="game in teamFutureGames" :key="game.Id">
      <td>{{game.Id}}</td>
      <td>
        <router-link :to="{ name: 'teamPage', params: { queryResults: game.HomeTeamId } }" >
        {{ teamInfo[game.HomeTeamId] }}
        </router-link>
      </td>
      <td>
        <router-link :to="{ name: 'teamPage', params: { queryResults: game.AwayTeamId } }" >
        {{ teamInfo[game.AwayTeamId] }}
        </router-link>
      </td>
      <td>{{ game.Date}}</td>
      <td>{{ game.Time }}</td>
      <td>{{ game.Stadium }}</td>
      <td>{{ game.referee }}</td>
      <div v-if="$root.store.username">
        <b-button @click="addFavorites(game.Id)" variant="primary"></b-button>
     </div> 
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
export default {
  name: "GamePreview",
   data() {
    return {
      teamInfo: {},
      favorites: [],
    };
  },
    props: {
    teamFutureGames:
    {
      required: true
      },
  }, 
    methods: {
    getTeamInfo: async function() {
      try {
         const teamInfo = await this.axios.get(`http://localhost:3000/allFutureGames`,)
         this.teamInfo= teamInfo.data;
          
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    addFavorites: 
    async function(id) {
      try {
        const exist = false
        for (const i in this.favorites) {
        if (id == this.favorites[i].Id) {
           exist= true;
        }
      }
      
    

        if (exist == true) {
          this.$root.toast("addToFavorites", "Game is already a favorites");
          return;
        }
        const response = await this.axios.post(
        'http://localhost:3000/users/favoriteGames',
        {gameId: id});
        this.$root.toast("addToFavorites", "Game has added to favourites!", "success");
        this.getFavorites();
      } catch (err) {
        console.log(err.response);
       
      }
    },

   
      
    getFavorites:
     async function() {
      try {
        const favoriteGames = await this.axios.get(
        'http://localhost:3000/users/favoriteGames'
        );
        for (const i in favoriteGames.data) {
          if (favoriteGames.data[i] != undefined)
           {
            this.favorites.push(favoriteGames.data[i]);
          }
        }
      } catch (err) {
        console.log(err.response);
      }
    }
  }
 
};
</script>



<!--need to add-->
<style>

</style>
