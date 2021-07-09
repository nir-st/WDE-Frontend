<template>
  <div class="container"> 
    <h3>Past Games</h3>
    <table>
        <tr>
            
          <th scope="col">Game ID</th>
          <th scope="col">Home Team</th>
          <th scope="col">Away Team</th>
          <th scope="col">Date</th>
          <th scope="col">Time</th>
          <th scope="col">Stadium</th>
          <th scope="col">Referee</th>
       </tr>
        <tbody>
          
        </tbody>
    </table>
  </div>
</template>

<script>
import GamePreview from "./GamePreview.vue";
export default {
  name: "PastGames",
  components: 
   
    {GamePreview},
 
  props: {
    pastGames: {
      type: Array,
      required: true
    }
  },
  
};
</script>

<style>
    table {
  /* font-family: 'Open Sans', sans-serif; */
  width: 90%;
  border-collapse: collapse;
  border: 3px solid #5c5f77;
  margin: 10px 10px 0 10px;
      font-family: monospace;

}

table th {
  text-transform: uppercase;
  text-align: left;
  background: #565970;
  color: #FFF;
  padding: 8px;
  min-width: 30px;
      font-family: monospace;

}

table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7D82A8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #D4D8F9;
}

.h3{
  font-family: monospace;
}
</style>