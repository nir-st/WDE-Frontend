<template>
<div>
  <table class="table table-dark">
  <thead>
    <tr>
      <th scope="col">Game ID</th>
      <th scope="col">Home Team</th>
      <th scope="col">Away Team</th>
      <th scope="col">Date</th>
      <th scope="col">Time</th>
      <th scope="col">Stadium</th>
      <th scope="col">Referee</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="Fgame in futureGames" :key="Fgame.id">
      
      <td>{{game.Id}}</td>
      <td>
        <router-link :to="{ name: 'teamPage', params: { queryResults: Fgame.LocalteamId } }" >
        {{ teamDict[Fgame.LocalteamId] }}
        </router-link>
      </td>
      <td>
        <router-link :to="{ name: 'teamPage', params: { queryResults: Fgame.VisitorteamId } }" >
        {{ teamDict[Fgame.VisitorteamId] }}
        </router-link>
      </td>
      <td>{{ Fgame.date.slice(0, 10) }}</td>
      <td>{{ Fgame.time.slice(11, 16) }}</td>
      <td>{{ Fgame.stadium }}</td>
      <td>{{ Fgame.referee }}</td>
     
      <div v-if="$root.store.username">
        <b-button @click="addToFavorites(Fgame.Id)" variant="primary">{{isFavorite(Fgame.Id) ? 'Favorite' : 'Add to Favorites'}}</b-button>
     </div> 
    </tr>
  </tbody>
</table>
  </div>
</template>


<script>
export default {
  name: "GamePreview",
  props: {
      id: {
        type: Number,
        required: true
      },
      hostTeam: {
        type: String,
        required: true
      },
      guestTeam: {
        type: String,
        required: true
      },
      date: {
        type: String,
        required: true
      },
      hour: {
        type: String,
        required: true
      }
  }, 
  
};
</script>

<style>
.game-preview {
  display: inline-block;
  width: 250px;
  height: 200px;
  position: relative;
  margin: 10px 10px;
  border-style: solid;
  border-radius: 10px;
  border-width: 5px;
  border-color:cadetblue;
}

.game-preview .game-title {
  text-align: center;
  text-transform: uppercase;
  color:  rgb(111, 197, 157);
}

.game-preview .game-content {
  width: 100%;
  overflow: hidden;
}



</style>













