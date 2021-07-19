<script>
export default {
  name: "Timer",
  data() {
    return {
      duration: 0,
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
      playerInitalValues: {
        duration: this.duration,
        minutes: 0,
        seconds: 0,
        isRunning: false,
        timer: null,
        isFirstMove: true,
      },
    };
  },
  methods: {
    startTimer() {
      // stop other player timer
      this.playerTwo.isRunning = false;
      clearInterval(this.playerTwo.timer);
      this.playerTwo.timer = null;
      // start timer
      let player = this.playerOne;
      let otherPlayer = this.playerTwo;
      player.isRunning = true;
      if (!player.isFirstMove) {
        otherPlayer.duration = otherPlayer.duration + parseInt(this.plus);
      }
      player.isFirstMove = false;
      if (!player.timer) {
        player.timer = setInterval(() => {
          if (player.duration > 0) {
            player.duration--;
          } else {
            this.isGameOver = true;
            this.winner = "playerOne";
            clearInterval(player.timer);
          }
        }, 1000);
      }
    },
    startTimerTwo() {
      this.playerOne.isRunning = false;
      clearInterval(this.playerOne.timer);
      this.playerOne.timer = null;
      let player = this.playerTwo;
      let otherPlayer = this.playerOne;
      player.isRunning = true;
      if (!player.isFirstMove) {
        otherPlayer.duration = otherPlayer.duration + parseInt(this.plus);
      }
      player.isFirstMove = false;
      if (!player.timer) {
        player.timer = setInterval(() => {
          if (player.duration > 0) {
            player.duration--;
          } else {
            this.isGameOver = true;
            this.winner = "playerTwo";
            clearInterval(player.timer);
          }
        }, 1000);
      }
    },
    resetTempo() {
      this.$router.go();
    },
    stop() {
      this.playerOne.isRunning = false;
      clearInterval(this.playerOne.timer);
      this.playerOne.timer = null;
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
  },
  computed: {
    compPlayerOne() {
      let duration = this.playerOne.duration;
      return {
        min: this.prettyTime(this.parseMins(duration)),
        sec: this.prettyTime(this.parseSecs(duration)),
      };
    },
    compPlayerTwo() {
      let duration = this.playerTwo.duration;
      return {
        min: this.prettyTime(this.parseMins(duration)),
        sec: this.prettyTime(this.parseSecs(duration)),
      };
    },
    setPlOneBgClasses() {
      return this.playerOne.duration < 10
        ? "danger"
        : this.playerOne.isRunning
        ? "active"
        : "";
    },
    setPlTwoBgClasses() {
      return this.playerTwo.duration < 10
        ? "danger"
        : this.playerTwo.isRunning
        ? "active"
        : "";
    },
  },
  created() {
    const tempo = this.$route.params.tempo.split("+");
    const minute = parseInt(tempo[0]) * 60;
    this.playerOne.duration = minute;
    this.playerTwo.duration = minute;
    this.plus = parseInt(tempo[1]);
    console.log(tempo);
  },
};
</script>
<template>
  <div class="timer">
    <button
      class="section"
      :disabled="playerTwo.isRunning"
      :class="setPlOneBgClasses"
      @click="startTimerTwo"
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
        {{ compPlayerOne.min }} : {{ compPlayerOne.sec }}
      </p>
    </button>
    <button
      class="section"
      :disabled="playerOne.isRunning"
      :class="setPlTwoBgClasses"
      @click="startTimer"
    >
      <div v-if="isGameOver && winner == 'playerTwo' " class="gameOver">
        <p>Diri kalamadın!</p>
        <button @click="resetTempo">Süreyi Yenile</button>
        <router-link to="/">
          Farklı tempo seç
        </router-link>
      </div>
      <p v-else class="time">
        {{ compPlayerTwo.min }} : {{ compPlayerTwo.sec }}
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
p.time {
  font-size: 90px;
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
  text-align: center;
}
.gameOver button {
  width: 100%;
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
