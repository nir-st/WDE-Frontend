<template>
  <div class="game-preview">
    <div :title="id" class="game-title">
      <b>Game Id:</b> {{ id }}
    </div>
    <ul class="game-content">
      <li> <b>host:</b> 
        <router-link :to="{name:'team', params:{id: this.hostTeamId} }">
          {{ this.hostTeamName }}
        </router-link>
      </li>
      <li> <b>guest:</b>
        <router-link :to="{name:'team', params:{id: this.hostTeamId} }">
          {{ this.guestTeamName }}
        </router-link>
      </li>
      <li> <b>date:</b> {{ date }}</li>
      <li> <b>time:</b> {{ hour }}</li>
      <li> <b>stadium:</b> {{ stadium }}</li>
      <li> <b>referee:</b> {{ referee }}</li>
    </ul>
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
      hostTeamId: {
        type: Number,
        required: true
      },
      guestTeamId: {
        type: Number,
        required: true
      },
      date: {
        type: String,
        required: true
      },
      hour: {
        type: String,
        required: true
      },
      stadium: {
        type: String,
        required: true
      },
      referee: {
        type: String,
        required: true
      }
  },
  data() {
    return {
      hostTeamName: "",
      guestTeamName: ""
    }
  },
  methods: {
    async getTeamNames() {
      const nameHome = await this.axios.get(
        `http://localhost:3000/teams/teamName/${this.hostTeamId}`
      );
      if (nameHome && nameHome.data) {
        this.hostTeamName = nameHome.data;
      }
      else {
        this.hostTeamName = "team name not found";
      }

      const nameAway = await this.axios.get(
        `http://localhost:3000/teams/teamName/${this.guestTeamId}`
      );
      if (nameAway && nameAway.data) {
        this.guestTeamName = nameAway.data;
      }
      else {
        this.guestTeamName = "team name not found";
      }
    }
  },
  mounted(){
    this.getTeamNames();
  } 
};
</script>

<style>
.game-preview {
  display: inline-block;
  width: 280px;
  height: 300px;
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
