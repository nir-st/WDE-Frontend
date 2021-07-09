<template>

  <div>
    <h3 class="title">All League Games</h3>
    <br/><br/>
    

    <Games
        :futureGames= "futureGames"
        :pastGames= "pastGames"
    ></Games>

  </div>

</template>

<script>
import Games from "../components/Games";
export default {
  components: { 
    Games
  },
  data() {
    return {
      pastGames: [],
      futureGames: []
    };
  },
  methods: {
    async getPastGames() {
      try {
        const PastGames = await this.axios.get(
        'http://localhost:3000/allPastGames'
        );
       
        for (const i in PastGames.data) {
          if (PastGames.data[i] != undefined) {
            
            this.pastGames.push(PastGames.data[i]);
            console.log(PastGames.data[i]);
          }
        }
        console.log(this.pastGames);
      } catch (err) {
        console.log(err.response);
    
      }
    },
    async getFutureGames() {
      try {
    
        const FutureGames = await this.axios.get(
        'http://localhost:3000/allFutureGames'
        );
        
        for (const i in FutureGames.data) {
          if (FutureGames.data[i] != undefined) {
            
            this.futureGames.push(FutureGames.data[i]);
          }
        }
      } catch (err) {
        console.log(err.response);
    
      }
    }
  },
  mounted(){
    this.getPastGames();
    this.getFutureGames();
  } 
};
</script>


<style scoped>
/* Style the buttons */
.title {
  display: flex;
  justify-content: center;
  color: black;
  font: sans-serif;
}
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