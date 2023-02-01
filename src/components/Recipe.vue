<script>
export default {
  data() {
    return {
      menu: [
        {
          name: "波波奶冻",
          img: "recipe-blancmange.jpg",
          ings: ["凉粉", "桂花茶", "椰汁", "麻薯粉", "蜂蜜水"],
        },
        {
          name: "奶酪蛋糕",
          img: "recipe-cheeseCake.jpg",
          ings: ["低筋面粉", "黄油", "奶酪", "淡奶油", "鸡蛋", "白砂糖"],
        },
        {
          name: "三明治",
          img: "recipe-sandwich.jpg",
          ings: ["方包", "青菜", "烤肠", "鸡蛋", "黄油", "芝士"],
        },
        {
          name: "红烧猪脚",
          img: "recipe-braisePigsFeet.jpg",
          ings: ["猪脚", "去腥料", "香辛料", "花生", "粟子"],
        },
        {
          name: "手撕包菜",
          img: "recipe-shreddedCabbage.jpg",
          ings: ["包菜", "粉丝", "香辛料", "浇汁"],
        },
        {
          name: "辣子鸡",
          img: "recipe-SpicyChicken.jpg",
          ings: ["鸡肉", "去腥料", "香辛料", "辣椒", "芝麻"],
        },
      ],
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    readIng(index) {
      this.showIng = true;
    },
    closeIng() {
      this.showIng = false;
    },
    init() {
      const menu = document.querySelectorAll(".menu");
      menu.forEach((item, index) => {
        setTimeout(() => {
          item.classList.add("show");
        }, index * 45 + 50);
      });
    },
  },
  setup() {
    const getUrl = (name) => {
      return new URL(`../assets/images/${name}`, import.meta.url).href;
    };
    return {
      getUrl,
    };
  },
};
</script>
<template>
  <div id="box">
    <div id="empty"></div>
    <div id="container">
      <div id="height">
        <div class="menu" v-for="list in menu" :key="list">
          <div class="name">
            {{ list.name }}
          </div>
          <img :src="getUrl(list.img)" class="img" />
          <div class="ings" v-for="ings in list.ings" :key="ings">
            {{ ings }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
#box {
  font-family: var(--ff-zhaijia);
  font-weight: 600;
  width: 100vw;
  height: 100vh;
  background: url("../assets/images/recipe-bg.jpg") no-repeat;
  background-size: cover;
  font-size: 0.9rem;
}
#empty,
#container {
  background-color: rgba(240, 195, 129, 0.3);
}
#empty {
  height: var(--btn-size);
}
#container {
  height: calc(100vh - var(--btn-size));
  padding-top: 10px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
  overflow: auto;
}
#height {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.menu {
  width: 24vw;
  min-width: 100px;
  max-width: 250px;
  margin: 2vh 2vw;
  padding: 2.5vw;
  border: var(--color-lightgrey) 1px dotted;
  border-radius: calc(var(--btn-size) / 2);
  background-color: var(--color-lightgrey);
  box-shadow: var(--shadow-white);
  opacity: 0;
}
.menu.show {
  animation: menuShow 0.6s linear forwards;
}
@keyframes menuShow {
  0% {
    opacity: 0;
    margin-left: calc(2vw + 5px);
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 1;
    margin-left: 2vw;
  }
}
.menu:hover {
  transform: scale(1.1);
  transition: all 0.2s ease-in-out;
}
.name,
.img,
.ings {
  width: 100%;
}
.name {
  text-align: center;
  text-shadow: var(--shadow-black);
  height: calc(var(--btn-size) / 1.5);
  line-height: calc(var(--btn-size) / 1.5);
  margin-bottom: 1vh;
  color: rgb(255, 244, 255);
  background-color: var(--color-blue);
  border-radius: calc(var(--btn-size) / 2.5);
}
.img {
  border-radius: calc(var(--btn-size) / 3.5);
  box-shadow: var(--shadow-white);
}
.ings {
  color: var(--color-blue);
  text-shadow: var(--shadow-white);
  padding: 5px 0;
  text-align: center;
}
</style>
