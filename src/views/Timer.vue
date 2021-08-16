<script>
export default {
  name: "ChessTimer",
  data() {
    return {
      plus: 0,
      winner: "",
      isGameOver: false,
      playerOne: {
        duration: 0,
        minutes: 0,
        seconds: 0,
        isRunning: false,
        timer: null,
        isFirstMove: true,
      },
      playerTwo: {
        duration: 0,
        minutes: 0,
        seconds: 0,
        isRunning: false,
        timer: null,
        isFirstMove: true,
      },
    };
  },
  methods: {
    handleStopTime(plyr) {
      const player = this[plyr];
      player.isRunning = false;
      clearInterval(player.timer);
      player.timer = null;
    },
    handlePlusDuration(plyr) {
      const player = this[plyr];
      if (!player.isFirstMove) {
        player.duration = parseInt(player.duration) + parseInt(this.plus);
      }
      player.isFirstMove = false;
    },
    handleCountDown(plyr) {
      const player = this[plyr];
      player.isRunning = true;
      if (!player.timer) {
        player.timer = setInterval(() => {
          if (player.duration > 0) {
            player.duration--;
          } else {
            this.isGameOver = true;
            this.winner = plyr;
            clearInterval(player.timer);
          }
        }, 1000);
      }
    },
    chessTimer(playerMadeMove, ActivePlayer) {
      this.handleStopTime(playerMadeMove);
      this.handlePlusDuration(playerMadeMove);
      this.handleCountDown(ActivePlayer);
    },
    resetTempo() {
      this.$router.go();
    },
    parseMins(minute) {
      return parseInt(minute / 60, 10);
    },
    parseSecs(seconds) {
      return parseInt(seconds % 60, 10);
    },
    prettyTime(time) {
      return time < 10 ? "0" + time : time;
    },
    setSelectedTempo() {
      const tempo = this.$route.params.tempo.split("+");
      const minute = parseInt(tempo[0]) * 60;
      this.playerOne.duration = minute;
      this.playerTwo.duration = minute;
      this.plus = parseInt(tempo[1]);
      
    },
    selm(){
      console.log('hey..');
    }
  },
  computed: {
    durationsPlayerOne() {
      let duration = this.playerOne.duration;
      return {
        min: this.prettyTime(this.parseMins(duration)),
        sec: this.prettyTime(this.parseSecs(duration)),
      };
    },
    durationsPlayerTwo() {
      let duration = this.playerTwo.duration;
      return {
        min: this.prettyTime(this.parseMins(duration)),
        sec: this.prettyTime(this.parseSecs(duration)),
      };
    },
    bgClassesPlayerOne() {
      return this.playerOne.duration < 11
        ? "danger"
        : this.playerOne.isRunning
        ? "active"
        : "";
    },
    bgClassesPlayerTwo() {
      return this.playerTwo.duration < 11
        ? "danger"
        : this.playerTwo.isRunning
        ? "active"
        : "";
    },
  },
  created() {
    this.setSelectedTempo();
  },
};
</script>
<template>
  <div class="timer">
    <button
      class="section rotate"
      :disabled="playerTwo.isRunning || isGameOver"
      :class="bgClassesPlayerOne"
      @click="chessTimer('playerOne', 'playerTwo')"
    >
      <div v-if="isGameOver && winner == 'playerOne'" class="gameOver">
        <p>Diri kalamadın!</p>
        <button @click="resetTempo">Süreyi Yenile</button>
        <router-link to="/">
          Farklı tempo seç
        </router-link>
      </div>
      <p v-else class="time">
        <router-link to="/" class="sendBack">GERİ</router-link>
        <span>{{ durationsPlayerOne.min }} : {{ durationsPlayerOne.sec }}</span>
      </p>
    </button>
    <button
      class="section"
      :disabled="playerOne.isRunning || isGameOver"
      :class="bgClassesPlayerTwo"
      @click="chessTimer('playerTwo', 'playerOne')"
    >
      <div v-if="isGameOver && winner == 'playerTwo'" class="gameOver">
        <p>Diri kalamadın!</p>
        <button @click="resetTempo">Süreyi Yenile</button>
        <router-link to="/">
          Farklı tempo seç
        </router-link>
      </div>
      <p v-else class="time">
        {{ durationsPlayerTwo.min }} : {{ durationsPlayerTwo.sec }}
      </p>
    </button>
  </div>
</template>

<style scoped>
.timer {
  max-width: 100vw;
  max-height: 100vh;
  min-width: 100vw;
  min-height: 100vh;
  display: grid;
  grid-auto-rows: 1fr 1fr;
  justify-items: center;
  align-items: center;
}
.section {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.section:first-child {
  border-bottom: 4px solid white;
}
.section.rotate p.time span{
  display: block;
  transform: rotate(180deg);
}
p.time {
  font-size: 110px;
  font-weight: bold;
}
.active {
  background: #66ac95;
}
.danger {
  background: rgb(209, 47, 47);
}
.gameOver {
  width: 100%;
  height: 100%;
  background: #191818;
  font-size: 45px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.gameOver button {
  /* width: 100%; */
  display: block;
  font-size: inherit;
  margin: 60px 0 40px 0;
}
.gameOver p {
  margin-top: 15px;
  font-size: 50px;
}

.sendBack {
  position: absolute;
  left: 20px;
  top: 20px;
  font-size: 24px;
}
</style>
