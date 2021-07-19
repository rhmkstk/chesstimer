<template>
  <div class="setup">
    <div class="wrapper">
      <div class="box" v-for="(tempo, index) in tempos" :key="index">
        <router-link :to="`/timer/${tempo.title}`" class="link">
          <span> {{ tempo.title }} </span>
        </router-link>
        
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Setup",
  data() {
    return {
      tempos: [
        { title: "1+0", minute: 3, plus: 0 },
        { title: "3+2", minute: 3, plus: 2 },
        { title: "5+0", minute: 5, plus: 0 },
        { title: "5+3", minute: 5, plus: 3 },
        { title: "10+0", minute: 10, plus: 0 },
        { title: "10+5", minute: 10, plus: 5 },
        { title: "15+10", minute: 15, plus: 10 },
        { title: "30+0", minute: 30, plus: 0 },
        { title: "Özel", minute: 6, plus: 4 },
      ],
    };
  },
};
</script>

<style>
.setup {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
}
.wrapper {
  height: max-content;
  width: max-content;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  gap: 20px;
}
.setup div.box {
  font-size: 30px;
  font-weight: bold;
  border: 1px solid white;
  border-radius: 10px;
  width: 120px;
  height: 80px;
  background: #191818;
}
div.box .link{
  width: 100%;
  height:100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
