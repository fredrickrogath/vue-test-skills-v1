<template>
  <header>
    <h1>Testing vue js Project 1</h1>
  </header>
  <div id="game">
    <section id="monster" class="container">
      <h2>Monster Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="monsterHealthStatus"></div>
      </div>
    </section>
    <section id="player" class="container">
      <h2>Your Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="playerHealthStatus"></div>
      </div>
    </section>
    <section class="container" v-if="winner">
      <h3 v-if="winner === 'monster'">You lost!</h3>
      <h3 v-else-if="winner === 'player'">You won!</h3>
      <h3 v-else>Draw</h3>
      <button @click="startNewGame">Start new game</button>
    </section>
    <section id="controls" v-if="!winner">
      <button @click="attackMonster">ATTACK</button>
      <button :disabled="mayUseSpecialAttack" @click="specialAttack">
        SPECIAL ATTACK
      </button>
      <button @click="healPlayer">HEAL</button>
      <button @click="surrender">SURRENDER</button>
    </section>
    <section id="log" class="container">
      <h2>Battle Log</h2>
      <ul v-for="(logMessage, key) in logMessages" :key="key">
        <span
          :class="{
            'log--player': logMessage.actionBy === 'player',
            'log--monster': logMessage.actionBy === 'monster',
          }"
        >
          {{ logMessage.actionBy }}
        </span>

        <span v-if="logMessage.actionType === 'heal'">
          heals for
          <span class="log--heal">
            {{ logMessage.actionValue }}
          </span>
        </span>

        <span v-else>
          attacks with
          <span class="log--damage">
            {{ logMessage.actionValue }}
          </span>
        </span>
      </ul>
    </section>
  </div>
</template>

<script>
function generateRandomValues(max, min) {
  return Math.floor(Math.random() * (max - min)) + min;
}

export default {
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      countRounds: 0,
      winner: null,
      logMessages: [],
    };
  },
  methods: {
    attackMonster() {
      this.countRounds++;
      const attackValue = generateRandomValues(12, 5);
      this.monsterHealth -= attackValue;
      this.attackPlayer();
      this.logMessage("player", "attack", attackValue);
    },
    attackPlayer() {
      const attackValue = generateRandomValues(15, 8);
      this.playerHealth -= attackValue;
      this.logMessage("monster", "attack", attackValue);
    },
    specialAttack() {
      this.countRounds++;
      const attackValue = generateRandomValues(25, 10);
      this.monsterHealth -= attackValue;
      this.logMessage("player", "speacial-attack", attackValue);
    },
    healPlayer() {
      const healValue = generateRandomValues(20, 8);
      if (this.playerHealth + healValue > 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += healValue;
      }

      this.attackPlayer();
      this.logMessage("player", "heal", healValue);
    },
    startNewGame() {
      this.monsterHealth = 100;
      this.playerHealth = 100;
      this.countRounds = 0;
      (this.logMessages = []), (this.winner = null);
    },
    surrender() {
      this.winner = "monster";
    },
    logMessage(who, what, value) {
      this.logMessages.unshift({
        actionBy: who,
        actionType: what,
        actionValue: value,
      });
    },
  },
  computed: {
    monsterHealthStatus() {
      if (this.monsterHealth <= 0) {
        return { width: "0%" };
      }
      return { width: this.monsterHealth + "%" };
    },
    playerHealthStatus() {
      if (this.playerHealth <= 0) {
        return { width: "0%" };
      }
      return { width: this.playerHealth + "%" };
    },
    mayUseSpecialAttack() {
      return this.countRounds % 3 !== 0;
    },
  },
  watch: {
    monsterHealth(value) {
      if (value <= 0 && this.playerHealth <= 0) {
        this.winner = "draw";
      } else if (value <= 0) {
        this.winner = "player";
      }
    },
    playerHealth(value) {
      if (value <= 0 && this.monsterHealth <= 0) {
        this.winner = "draw";
      } else if (value <= 0) {
        this.winner = "monster";
      }
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0.5rem;
  background-color: #880017;
  color: white;
  text-align: center;
  margin-bottom: 2rem;
}

section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}

#monster h2,
#player h2 {
  margin: 0.25rem;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #7700ff;
}

.log--monster {
  color: #da8d00;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>