<template>
  <div class='Container'>
    <div class='title'>
      <img src="../assets/logo.png" />
    </div>
    <div class="playersContainer">
      <div v-for="(guy, n) in JSON.parse(guys)" :id="n" :key="n" class="player">
        <div class="nameContainer">
          <p>{{ guy.name }}</p>
          <button v-on:click="deleteUser(guy.name)">Supprimer</button>
        </div>
        <div class="scoreContainer">
          <span class="destroyed">
            <button v-on:click="updateUser(n, guy.name, guy.destroyed + 1, guy.missed)"
              style='background-color:green;border-radius:30%'><img src="../assets/target-icon.png"
                style='height:70px;' /></button> {{
                    guy.destroyed
                }}
          </span>
          <p style='font-size:80px;padding-bottom:20px'>-</p>
          <span class="missed">
            {{ guy.missed
            }}
            <button v-on:click="updateUser(n, guy.name, guy.destroyed, guy.missed + 1)"
              style='background-color:darkred;border-radius:30%'><img src="../assets/targetmiss-icon.png"
                style='height:70px;' /></button>
          </span>
        </div>
      </div>
    </div>

    <div class="optionContainer">
      <div class="inpute">
        <input v-model="name" type="text" placeholder="Prénom.." v-on:keydown.enter="storeNewUser"
          v-on:keyup.enter="handleInput($event)" />
      </div>
      <div class="lesbutton">
        <button class="button" v-on:click="storeNewUser">Ajouter un joueur</button>
        <button class="button" v-on:click="clearScores">Remettre à 0</button>
        <button class="button" v-on:click="newGame">Nouvelle partie</button>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "IndexPage",

  data() {
    return {
      guys: localStorage.getItem('guys'),
      name: ''
    }
  },

  mounted() {
    if (JSON.parse(localStorage.getItem('guys')) === undefined || JSON.parse(localStorage.getItem('guys')) === null) {
      localStorage.setItem('guys', JSON.stringify([]));
    }
  },

  beforeDestroy() { },

  methods: {
    newGame() {
      localStorage.setItem('guys', JSON.stringify([]));
      this.guys = JSON.stringify([]);
    },
    clearScores() {
      const list = JSON.parse(this.guys);
      const newList = list.map(player => {
        player = { name: player.name, destroyed: 0, missed: 0 }
        return player;
      });
      localStorage.setItem('guys', JSON.stringify(newList));
      this.guys = JSON.stringify(newList);
    },
    handleInput(e) {
      e.target.value = '';
      this.name = '';
    },
    storeNewUser(e) {
      if (process.client) {
        const list = JSON.parse(localStorage.getItem('guys'));
        list.push({
          name: this.name,
          destroyed: 0,
          missed: 0
        });
        localStorage.setItem('guys', JSON.stringify(list))
        this.guys = JSON.stringify(list)
        this.name = ''
      }
    },
    updateUser(n, guyName, dest, miss) {
      const list = JSON.parse(this.guys);
      list[n] = { name: guyName, destroyed: dest, missed: miss };
      this.guys = JSON.stringify(list);
      localStorage.setItem('guys', JSON.stringify(list));
    },
    deleteUser(name) {
      const list = JSON.parse(this.guys);
      const newList = list.filter(guy => guy.name !== name);
      this.guys = JSON.stringify(newList);
      localStorage.setItem('guys', JSON.stringify(newList));
    }
  },
}
</script>
