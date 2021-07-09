<template>
  <div>
    <h2>Future Games:</h2>
    <futureGames
      :FutureGames ="future" 
    ></futureGames>
    <h2>Past Games</h2>
    <pastGames
      :PastGames="past" 
    ></pastGames>
   
    
  </div>
</template>

<script>
import futureGames from "../components/FutureGames";
import pastGames from "../components/PastGames";
export default {
  name: "Games",
  components: {
    futureGames,
    pastGames
  }, 
  props: {
    future: {
      type: Array,
      required: true
      },
    past: {
      type: Array,
      required: true
      }
  },
};
</script>

<style></style>



