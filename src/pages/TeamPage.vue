<template>
  <div class="div">
    <center>
      <b-card no-body class="overflow-hidden" style="max-width: 540px;" bg-variant="light">
        <b-row no-gutters>
          <b-col md="6">
            <b-card-img v-bind:src="this.logoPath" alt="Image" class="rounded-0"></b-card-img>
          </b-col>
          <b-col md="6">
            <b-card-body v-bind:title="this.name">
              <b-card-text>
                Founded in {{this.founded}}
              </b-card-text>
            </b-card-body>
          </b-col>
        </b-row>
      </b-card>
    <div class='data'>
      <b-card no-body>
        <b-tabs pills card vertical>
          <b-tab title="Players" active><b-card-text>
            <div style="height:450px;border:1px solid #ccc;font:16px/26px Georgia, Garamond, Serif;overflow:auto;">
              <div class=player v-for="player in this.players" :key="player.id">
                <router-link :to="{name:'player', params:{id: player.id} }">
                  <PlayerPreview
                    :fullname=player.fullname
                    :teamname=player.teamName
                    :imageUrl=player.image_url
                    :position=player.positionId
                  ></PlayerPreview>
                </router-link>
              </div>
            </div>
          </b-card-text></b-tab>
          <b-tab title="Past Games"><b-card-text>
            <div style="height:450px;border:1px solid #ccc;font:16px/26px Georgia, Garamond, Serif;overflow:auto;">
              Past games here
            </div>
          </b-card-text></b-tab>
          <b-tab title="Future Games"><b-card-text>
            <div style="height:450px;border:1px solid #ccc;font:16px/26px Georgia, Garamond, Serif;overflow:auto;">
              Future games here
            </div>
          </b-card-text></b-tab>
        </b-tabs>
      </b-card>
    </div>
    </center>
  </div>
</template>

<script>
import PlayerPreview from '../components/PlayerPreview.vue';
export default {
  components: {
      PlayerPreview
  },
  props: {
    id: Number
  },
  data() {
    return {
      name: "",
      founded: "",
      logoPath: "",
      players: []
    }
  },
  methods: {
    async getData() {
        const response = await this.axios.get(
        `http://localhost:3000/teams/teamFullDetails/${this.$route.params.id}`
        );
        this.name = response.data.name;
        this.founded = response.data.founded;
        this.logoPath = response.data.logoPath;
        this.players = response.data.players;
        console.log(response);
        // this.name = "Silkeborg";
        // this.logoPath = "https://cdn.sportmonks.com/images//soccer/teams/22/86.png";
        // this.founded = 2000;
        // this.players = [
        //     {
        //         "id": 527450,
        //         "fullname": "Patrik Sigurður Gunnarsson",
        //         "commonName": "P. Gunnarsson",
        //         "teamName": "Brentford",
        //         "positionId": 1,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "15/11/2000",
        //         "birthCountry": "Iceland",
        //         "nationality": "Iceland",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 84907,
        //         "fullname": "Oscar Hedvall",
        //         "commonName": "O. Hedvall",
        //         "teamName": "Silkeborg",
        //         "positionId": 1,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/11/84907.png",
        //         "birthDate": "09/08/1998",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "195 cm",
        //         "weight": "84 kg"
        //     },
        //     {
        //         "id": 37547204,
        //         "fullname": "Esben Lange",
        //         "commonName": "E. Lange",
        //         "teamName": "Silkeborg",
        //         "positionId": 1,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": null,
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 84175,
        //         "fullname": "Thomas Nørgaard",
        //         "commonName": "T. Nørgaard",
        //         "teamName": "Silkeborg",
        //         "positionId": 1,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/15/84175.png",
        //         "birthDate": "07/01/1987",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "185 cm",
        //         "weight": "77 kg"
        //     },
        //     {
        //         "id": 29091,
        //         "fullname": "Stan van Bladeren",
        //         "commonName": "S. van Bladeren",
        //         "teamName": "Silkeborg",
        //         "positionId": 1,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "01/10/1997",
        //         "birthCountry": "Netherlands",
        //         "nationality": "Netherlands",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 37546910,
        //         "fullname": "Alexander Magnus Busch",
        //         "commonName": "A. Busch",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": null,
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 85731,
        //         "fullname": "Joel Felix",
        //         "commonName": "J. Felix",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "13/01/1998",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 24818225,
        //         "fullname": "Oscar Fuglsang",
        //         "commonName": "O. Fuglsang",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "12/03/2002",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 86034,
        //         "fullname": "Anders Hagelskjær",
        //         "commonName": "A. Hagelskjær",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/18/86034.png",
        //         "birthDate": "16/02/1997",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "187 cm",
        //         "weight": null
        //     },
        //     {
        //         "id": 25797,
        //         "fullname": "Milan Massop",
        //         "commonName": "M. Massop",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/5/25797.png",
        //         "birthDate": "01/12/1993",
        //         "birthCountry": "Netherlands",
        //         "nationality": "Netherlands",
        //         "height": "188 cm",
        //         "weight": "75 kg"
        //     },
        //     {
        //         "id": 84605,
        //         "fullname": "Tobias Salquist",
        //         "commonName": "T. Salquist",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "18/05/1995",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 84616,
        //         "fullname": "Jeppe Gertsen",
        //         "commonName": "J. Gertsen",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/8/84616.png",
        //         "birthDate": "09/02/1997",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "188 cm",
        //         "weight": "79 kg"
        //     },
        //     {
        //         "id": 37527760,
        //         "fullname": "Anders Sønderby",
        //         "commonName": "A. Sønderby",
        //         "teamName": "Silkeborg",
        //         "positionId": 2,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": null,
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 84621,
        //         "fullname": "Mark Brink Christensen",
        //         "commonName": "M. Brink Christensen",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/13/84621.png",
        //         "birthDate": "15/03/1998",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "177 cm",
        //         "weight": "72 kg"
        //     },
        //     {
        //         "id": 24818231,
        //         "fullname": "Frederik Carstensen",
        //         "commonName": "F. Carstensen",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "09/04/2002",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 32796892,
        //         "fullname": "Rasmus Carstensen",
        //         "commonName": "R. Carstensen",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/28/32796892.png",
        //         "birthDate": "11/10/2000",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "187 cm",
        //         "weight": "76 kg"
        //     },
        //     {
        //         "id": 37540286,
        //         "fullname": "Anders Dahl",
        //         "commonName": "A. Dahl",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "01/05/2002",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 3533801,
        //         "fullname": "Magnus Mattsson",
        //         "commonName": "M. Mattsson",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/9/3533801.png",
        //         "birthDate": "25/02/1999",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "174 cm",
        //         "weight": "62 kg"
        //     },
        //     {
        //         "id": 21762796,
        //         "fullname": "Sebastian Vinther Jørgensen",
        //         "commonName": "S. Jørgensen",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/12/21762796.png",
        //         "birthDate": "08/06/2000",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "183 cm",
        //         "weight": "73 kg"
        //     },
        //     {
        //         "id": 85909,
        //         "fullname": "Mads Kaalund Larsen",
        //         "commonName": "M. Kaalund Larsen",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/21/85909.png",
        //         "birthDate": "16/08/1996",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "182 cm",
        //         "weight": "71 kg"
        //     },
        //     {
        //         "id": 37546909,
        //         "fullname": "Jeppe Kilden Grøn",
        //         "commonName": "J. Kilden Grøn",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": null,
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 84599,
        //         "fullname": "Gustav Klitgård Dahl",
        //         "commonName": "G. Klitgård Dahl",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/23/84599.png",
        //         "birthDate": "21/01/1996",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "181 cm",
        //         "weight": "71 kg"
        //     },
        //     {
        //         "id": 37518836,
        //         "fullname": "Anders Ferslev Klynge",
        //         "commonName": "A. Klynge",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "14/10/2000",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 33576939,
        //         "fullname": "Pelle Elkjær Mattsson",
        //         "commonName": "P. Mattsson",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/11/33576939.png",
        //         "birthDate": "04/08/2001",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "174 cm",
        //         "weight": "62 kg"
        //     },
        //     {
        //         "id": 1548358,
        //         "fullname": "Valance Nambishi",
        //         "commonName": "V. Nambishi",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/6/1548358.png",
        //         "birthDate": "30/11/1997",
        //         "birthCountry": "Namibia",
        //         "nationality": "Namibia",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 84557,
        //         "fullname": "Nicklas Røjkjær",
        //         "commonName": "N. Røjkjær",
        //         "teamName": "Silkeborg",
        //         "positionId": 3,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/13/84557.png",
        //         "birthDate": "24/07/1998",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "179 cm",
        //         "weight": "68 kg"
        //     },
        //     {
        //         "id": 20694538,
        //         "fullname": "Nicolai Vallys",
        //         "commonName": "N. Vallys",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/10/20694538.png",
        //         "birthDate": "04/09/1996",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 12036308,
        //         "fullname": "Wessam Abou Ali",
        //         "commonName": "W. Abou Ali",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/20/12036308.png",
        //         "birthDate": "04/01/1999",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "186 cm",
        //         "weight": null
        //     },
        //     {
        //         "id": 954,
        //         "fullname": "Nicklas Helenius",
        //         "commonName": "N. Helenius",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/26/954.png",
        //         "birthDate": "08/05/1991",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": "195 cm",
        //         "weight": "84 kg"
        //     },
        //     {
        //         "id": 85659,
        //         "fullname": "Emil Holten",
        //         "commonName": "E. Holten",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/27/85659.png",
        //         "birthDate": "08/08/1996",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 24818234,
        //         "fullname": "Alexander Rasmussen",
        //         "commonName": "A. Rasmussen",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/players/26/24818234.png",
        //         "birthDate": "26/06/2002",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 32786905,
        //         "fullname": "Niclas Holm Pedersen",
        //         "commonName": "N. Pedersen",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "15/03/2002",
        //         "birthCountry": "Denmark",
        //         "nationality": "Denmark",
        //         "height": null,
        //         "weight": null
        //     },
        //     {
        //         "id": 119637,
        //         "fullname": "Stefán Teitur Þórðarson",
        //         "commonName": "S. Þórðarson",
        //         "teamName": "Silkeborg",
        //         "positionId": 4,
        //         "image_url": "https://cdn.sportmonks.com/images/soccer/placeholder.png",
        //         "birthDate": "16/10/1998",
        //         "birthCountry": "Iceland",
        //         "nationality": "Iceland",
        //         "height": null,
        //         "weight": null
        //     }
        // ]
    }
  },
  mounted() {
    this.getData()
  },
}
</script>

<style scoped>
.div {
    margin-top: 30px;
    color: black;
}

.data {
    margin-top: 30px;
    width: 75%;
}

.player {
    float: left;
    margin-right: 20px;
    margin-left: 20px;
    margin-bottom: 20px;
    width: 250px;
    height: 500px;
}
</style>