<template>

  <div>
    <h1 class="title">Favorite Games Page</h1>
    <br/><br/>

    <FavoriteGames v-bind:games="games"></FavoriteGames>

  </div>

</template>

<script>
import FavoriteGames from "../components/FavoriteGames";
export default {
  components: { 
    FavoriteGames
  }, 
  data() {
    return {
      games: []
    };
  },
  methods: {
    getFavoriteGames: async function() {
      try {
        
        const favoriteGames = await this.axios.get(
        'http://localhost:3000/users/favoriteGames'
        );
        for (const i in favoriteGames.data) {
          if (favoriteGames.data[i] != undefined) {
           
            this.games.push(favoriteGames.data[i]);
          }
        }
      } catch (err) {
        console.log(err.response);
      }
    }
  },
  mounted(){
    this.getFavoriteGames();
  } 
};
</script>


<style scoped>
/* Style the buttons */
.btn {
  border: none;
  outline: none;
  padding: 10px 16px;
  background-color: #f1f1f1;
  cursor: pointer;
}

/* Style the active class (and buttons on mouse-over) */
.active, .btn:hover {
  background-color: #666;
  color: white;
}
#search-input {
  margin-left: 20px; 
  width: 500px; 
}
</style>