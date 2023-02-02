<script>
export default {
  data() {
    return {
      text: [
        "Hi，这里是棋儿",
        "一个喜欢写代码的青年",
        "\n",
        "通过自学搭建了这个小博客",
        "emm..我更喜欢称之为基地啦",
        "\n",
        "对这里还满意吗 :)",
        "\n",
        "二维码已奉上，来联系我吧",
        "验证消息请输入'前来集结'",
        "否则信号会在宇宙中迷失哦 ：(",
        "\n",
        "期待与你相遇 ๑'ꀾ'๑",
      ],
      code: false,
      disclaimer:
        "博客部分图片收集于网络，仅供个人学习和欣赏使用。\n版权归原作者（含AI）所有。\n如侵犯了您的版权，请联系我删除并向您致歉。",
    };
  },
  mounted() {
    this.addShow();
    setTimeout(() => {
      this.delShow();
    }, 800);
  },
  methods: {
    codeT() {
      this.code = !this.code;
    },
    addShow() {
      const qr = document.querySelector("#qr");
      qr.classList.add("show");
    },
    delShow() {
      const qr = document.querySelector("#qr");
      qr.classList.remove("show");
    },
  },
};
</script>
<template>
  <div id="box">
    <div id="empty"></div>
    <div id="container">
      <div id="logo"></div>
      <div id="intr">
        <p v-for="text in text" :key="text">
          {{ text }}
        </p>
      </div>
      <transition name="fadeQr"
        ><div id="qr" v-show="!code" @click="codeT">
          点击此处联系我
        </div></transition
      >
      <transition name="fadeCode"
        ><img
          src="../assets/images/aboutMe-code.png"
          alt=""
          id="code"
          v-show="code"
          @click="codeT"
      /></transition>
      <div id="footer" v-show="!code">
        {{ disclaimer }}
      </div>
    </div>
  </div>
</template>
<style scoped>
#box {
  width: 100vw;
  height: 100vh;
  background-image: url("../assets/images/aboutMe-bg.jpg");
  background-size: cover;
  background-position: right;
  font-size: 0.9rem;
  font-family: var(--ff-zhaijia);
  color: rgb(17, 46, 36);
}
#empty,
#container {
  background-color: rgba(221, 223, 151, 0.3);
}
#empty {
  height: var(--btn-size);
}
#container {
  height: calc(100vh - var(--btn-size));
  padding-top: 10vh;
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
}
#logo {
  width: 120px;
  height: 120px;
  background-image: url("../assets/images/aboutMe-logo.jpg");
  background-size: cover;
  border-radius: 50%;
  border: #fff 8px dotted;
  background-clip: padding-box;
  transition: all 1s ease-in-out;
  animation: around 10s linear infinite;
}
@keyframes around {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
#intr {
  text-align: center;
  margin: 30px 0;
  padding: 10px 30px;
  border-radius: 30px;
  background-color: var(--color-lightgrey);
  white-space: pre-wrap;
  box-shadow: var(--shadow-white);
  position: relative;
  opacity: 0;
  transition: all 1s ease-in-out;
  animation: down 0.5s linear forwards;
}
#intr p {
  margin-top: 5px;
}
@keyframes down {
  0% {
    opacity: 0;
    transform: translateY(-2%);
  }
  100% {
    opacity: 1;
    transform: none;
  }
}
#qr {
  text-align: center;
  padding: 5px 10px;
  border-radius: 10px;
  background-color: var(--color-lightgrey);
  box-shadow: var(--shadow-white);
  cursor: pointer;
}
#qr.show {
  opacity: 0;
  transition: all 1s ease-in-out;
  animation: down 0.5s 0.2s ease-in-out forwards;
}
#code {
  width: 200px;
}
.fadeInt-enter-active,
.fadeQr-enter-active,
.fadeCode-enter-active {
  opacity: 0;
  transform: translateY(-2%);
}

.fadeInt-enter-to,
.fadeQr-enter-to,
.fadeCode-enter-to {
  opacity: 1;
  transform: none;
  transition: all 0.5s ease-in-out;
}
#footer {
  width: 100%;
  color: var(--color-lightgrey);
  scale: 0.6;
  text-align: center;
  line-height: 1.3rem;
  position: fixed;
  bottom: 5px;
  white-space: pre-wrap;
}
</style>
