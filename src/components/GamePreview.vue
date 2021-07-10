<template>
  <div class="game-preview">
    <div :title="id" class="game-title">
      <b>Game Id:</b> {{ id }}
    </div>
    <ul class="game-content">
      <li v-if="!this.isPast"> <b>host:</b> 
        <router-link :to="{name:'team', params:{id: this.hostTeamId} }">
          {{ this.hostTeamName }}
        </router-link>
      </li>
      <li v-if="!this.isPast" > <b>guest:</b>
        <router-link :to="{name:'team', params:{id: this.hostTeamId} }">
          {{ this.guestTeamName }}
        </router-link>
      </li>
      <li v-if="this.isPast"> <b>host:</b> 
        <router-link :to="{name:'team', params:{id: this.hostTeamId} }">
          {{ this.hostTeamName }}
        </router-link>
        ({{ this.homeTeamScore }})
      </li>
      <li v-if="this.isPast" > <b>guest:</b>
        <router-link :to="{name:'team', params:{id: this.hostTeamId} }">
          {{ this.guestTeamName }}
        </router-link>
        ({{ this.awayTeamScore }})
      </li>
      <li> <b>date:</b> {{ date }}</li>
      <li> <b>time:</b> {{ hour }}</li>
      <li> <b>stadium:</b> {{ stadium }}</li>
      <li> <b>referee:</b> {{ referee }}</li>
      <br/>
      <b-form-checkbox 
        v-if="(!this.isPast) && this.isLogged"
        switch size="lg"
        v-model="isFavorite"
        @change="update"
      >
        Favorite
      </b-form-checkbox>
      <b-button v-if="this.isPast" v-bind:id="this.btnDataVal()">
        Game Events
      </b-button>
      <b-popover v-if="this.isPast" v-bind:target="this.btnDataVal()" triggers="hover" placement="bottomright">
        <template #title>Game Event</template>
          <ul>
            <li v-for="event in eventLog" :key="event.Description">
              {{ event.Description }} ({{ event.GameMinute }})
            </li>
          </ul>
      </b-popover>
    </ul>
  </div>
</template>

<script>
export default {
  name: "GamePreview",
  props: {
      isPast: {
        type: Boolean,
        require: true
      },
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
      },
      homeTeamScore: {
        type: Number,
        required: false
      },
      awayTeamScore: {
        type: Number,
        required: false
      },
      eventLog: {
        type: Array,
        required: false
      }
  },
  data() {
    return {
      hostTeamName: "",
      guestTeamName: "",
      isFavorite: false
    }
  },
  computed: {
    isLogged: function() {
      if (localStorage.getItem('username') === null) {
        console.log('nooo')
        return false;
      }
      console.log('yessss')
      return true;
    }
  },
  methods: {
    btnDataVal() {
      return this.id.toString()
    },
    async update() {
      if (this.isFavorite == true) {
        console.log('adding to favorites!')
        const res = await this.axios.post(
          `http://localhost:3000/users/favoriteGames`,
          {
            gameId: this.id,
            username: this.$root.store.username
          }
        )
      }
      else {
        console.log('removing from favorites!')
        console.log('username is ' + this.$root.store.username)
        const res = await this.axios.delete(
          `http://localhost:3000/users/favoriteGames`,
          {
            data: {
              gameId: this.id,
              username: this.$root.store.username
            }
          }
        )
      }
      console.log('game preview emitting')
      this.$emit('favoriteUpdatedEvent')
    },
    async checkIsFavorite() {
      const username = this.$root.store.username;
      const res = await this.axios.get(
        `http://localhost:3000/users/favoriteGames/${username}`,
      )
      if (res && res.data && res.data.length > 0) {
        res.data.forEach((game) => {
          if (game.GameId == this.id) {
            this.isFavorite = true;
          }
        })
      }
    },
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
  mounted() {
    this.getTeamNames();
    if ((!this.isPast) && this.$root.store.username) {
      this.checkIsFavorite();
    }
  },
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
