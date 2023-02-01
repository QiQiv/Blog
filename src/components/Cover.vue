<script>
export default {
  data() {
    return {
      typerLetter: "机械少女降临企划",
      readme: [
        "(ෆ`꒳´ෆ)",
        "Hi~ ",
        "欢迎加入",
        "基地编号 Q i i 7 2 2",
        "这里有小工具、小游戏",
        "当然还可以联系我进行基地建设",
        "从上方开始探索吧",
        "祝你旅途愉快",
        "(ෆ`꒳´ෆ)",
      ],
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      const letter = this.typerLetter;
      const typer = document.querySelector("#typer");
      setTimeout(() => {
        const readText = document.querySelectorAll(".readText");
        readText.forEach((e, i) => {
          setTimeout(() => {
            e.classList.add("show");
          }, i * 450);
        });
      }, 500);
      for (let i = 1; i <= letter.length; i++) {
        setTimeout(() => {
          typer.innerText = letter.substring(0, i);
        }, i * 150);
      }
    },
  },
};
</script>

<template>
  <div id="box">
    <div id="empty"></div>
    <div id="container">
      <div id="content">
        <h2 id="typer"></h2>
        <div id="readme">
          <h3 v-for="item in readme" :key="item" class="readText">
            {{ item }}
          </h3>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
#box {
  width: 100vw;
  height: 100vh;
  background: url("../assets/images/index-bg.jpg") no-repeat;
  background-size: cover;
  color: var(--color-lightgrey);
  font-family: var(--ff-hutu);
}
#empty {
  background-color: rgba(31, 8, 66, 0.4);
  height: var(--btn-size);
}
#container {
  width: 100%;
  height: calc(100vh - var(--btn-size));
  background-color: rgba(31, 8, 66, 0.4);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
#content {
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  text-shadow: var(--shadow-white);
}
#typer {
  height: 10vh;
  font-size: 2rem;
}
#readme {
  height: 50vh;
}
.readText {
  padding: 1vh 0;
  opacity: 0;
}
.readText.show {
  animation: readTextShow 0.6s forwards;
}
@keyframes readTextShow {
  0% {
    opacity: 0;
    margin-top: 1vh;
  }
  100% {
    opacity: 1;
  }
}
</style>
