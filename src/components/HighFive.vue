<script>
let paw = null;
let pawer = null;
let timer = null;
export default {
  data() {
    return {
      music: {
        start: "game-start.mp3",
        bgm: "game-bgm.mp3",
        caugth: "game-caugth.mp3",
      },
      cover: {
        title: "抓ฅ^ↀᴥↀ^ฅ",
        dspt: ["点击猫爪消除", "抓住全部猫猫即可胜利", "准备好就开始吧 \u21E9"],
        startBtn: "\u25BA 开始冒险 \u25C4",
      },
      coverShow: true,
      min: 0,
      sec: 0,
      score: 0,
      pawCounter: 0,
      end: false,
    };
  },
  methods: {
    start() {
      this.coverShow = !this.coverShow;
      const musStr = document.querySelector("#musStr");
      const musBgm = document.querySelector("#musBgm");
      musStr.play();
      musStr.volume = 0.1;
      this.pawCreate();
      setTimeout(() => {
        musBgm.play();
        musBgm.volume = 0.1;
      }, 600);
      pawer = window.setInterval(() => {
        this.pawCreate();
      }, 700);
    },
    timeCounter() {
      timer = window.setInterval(() => {
        if (this.sec == 60) {
          this.min++;
          this.sec = 0;
        } else {
          this.sec++;
        }
      }, 1000);
    },
    addClick() {},
    pawCreate() {
      const game = document.querySelector("#game");
      if (game) {
        paw = document.createElement("img");
        paw.src = this.getUrl("highFive-paw.png");
        paw.classList.add("paws");
        const { x, y } = this.pawLocat();
        paw.style.top = `${y}px`;
        paw.style.left = `${x}px`;
        paw.style.transform = `rotate(${Math.random() * 360}deg)`;
        paw.addEventListener("click", this.pawCatch, false);
        game.appendChild(paw);
        this.pawCounter++;
      }
    },
    pawCatch(e) {
      const delPaw = e.target;
      const musCat = document.querySelector("#musCat");
      musCat.play();
      musCat.volume = 0.3;
      delPaw.remove();
      this.pawCounter--;
      this.score++;

      if (this.pawCounter == 0) {
        clearInterval(pawer);
        clearInterval(timer);
        this.win();
      }
    },
    pawLocat() {
      const width = document.querySelector("#game").clientWidth;
      const height = document.querySelector("#game").clientHeight;
      let x = Math.random() * width;
      let y = Math.random() * height;
      width - x < 100 ? (x -= 100) : x;
      height - y < 100 ? (y -= 100) : y;
      return { x, y };
    },
    win() {
      this.end = true;
    },
  },

  computed: {
    second() {
      return this.sec;
    },
    minute() {
      return this.min;
    },
    scr() {
      return this.score;
    },
  },
  setup() {
    const getUrl = (name) => {
      return new URL(`../assets/images/${name}`, import.meta.url).href;
    };
    const getMus = (name) => {
      return new URL(`../assets/music/${name}`, import.meta.url).href;
    };
    return {
      getUrl,
      getMus,
    };
  },
};
</script>
<template>
  <div id="box">
    <div id="empty"></div>
    <div id="container">
      <audio :src="getMus(music.bgm)" loop id="musBgm"></audio>
      <audio :src="getMus(music.caugth)" id="musCat"></audio>
      <audio :src="getMus(music.start)" id="musStr"></audio>
      <div class="screen" id="cover" v-if="coverShow">
        <div id="title">{{ cover.title }}</div>
        <div id="dspt">
          <div v-for="item in cover.dspt" :key="item">{{ item }}</div>
        </div>
        <div id="startBtn" @click="start(), timeCounter()">
          {{ cover.startBtn }}
        </div>
      </div>

      <div class="screen" id="playing" v-if="!coverShow">
        <div id="counter">
          <div id="time">
            时间 <span id="min">{{ min }}</span> :
            <span id="sec">{{ sec }}</span>
          </div>
          <div id="scr">
            得分 <span id="score">{{ score }}</span>
          </div>
        </div>

        <div id="game" v-if="!end"></div>

        <div id="end" v-if="end">
          <img src="../assets/images/highFive-win.gif" alt="" class="gif" />
          <div id="win">
            太棒啦！你打破了历史纪录！<br />
            用时： {{ min }} 分 {{ sec }} 秒 <br />
            成功与 {{ score }} 只猫猫击掌
          </div>
          <img src="../assets/images/highFive-win.gif" alt="" class="gif" />
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
#box {
  width: 100vw;
  height: 100vh;
  font-family: var(--ff-hutu);
  background: url("../assets/images/highFive-bg.jpg") no-repeat;
  background-size: cover;
}
#empty,
#container {
  background-color: var(--color-pink);
}
#empty {
  height: var(--btn-size);
}
#container {
  height: calc(100vh - var(--btn-size));
  color: rgb(243, 199, 233);
  text-shadow: var(--shadow-white);
  font-size: 1.2rem;
}
.screen {
  bottom: 0;
  width: 100%;
  height: calc(100vh - var(--btn-size));
  overflow: hidden;
}
#cover {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}
#cover::after {
  content: "";
  width: 25vh;
  height: 25vh;
  border-radius: 50%;
  border: #fff 12px dotted;
  background-color: rgba(255, 255, 255, 0.9);
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 0;
  background-clip: padding-box;
  animation: around 24s linear infinite;
}
@keyframes around {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}
#title,
#dspt,
#startBtn {
  text-align: center;
  margin: 0.6vh 0;
  z-index: 1;
}
#title {
  color: rgb(223, 160, 208);
  font-size: 2rem;
}
#dspt div {
  margin: 2px 0;
}
#startBtn {
  font-size: 1.3rem;
  cursor: pointer;
  color: rgb(219, 141, 201);
  text-shadow: var(--shadow-purple);
}
#startBtn:hover {
  transform: scale(1.1);
  transition: all 0.1s ease-in-out;
}
#playing {
  color: rgb(245, 158, 225);
  position: relative;
  display: flex;
  flex-direction: column;
}
#counter {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
#time,
#scr {
  font-family: var(--ff-zhaijia);
  font-size: 1.2rem;
  color: #fff;
  padding: 5px 15px;
  margin: 1vh auto;
}
#game {
  width: 100%;
  height: 100%;
  position: relative;
}
#end {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
#win {
  text-align: center;
  font-weight: bolder;
  line-height: 2rem;
  font-family: var(--ff-zhaijia);
  color: #fff;
}
.gif {
  width: 80px;
}
</style>
<style>
.paws {
  width: 100px;
  height: 100px;
  cursor: pointer;
  position: absolute;
}
</style>
