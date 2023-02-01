<script>
export default {
  data() {
    return {
      menuL: ["首页", "菜谱收藏", "小游戏"],
      menuR: ["排班工具", "图库收藏", "关于我"],
      iconL: ["reorder-three", "restaurant", "game-controller"],
      iconR: ["person-circle", "images", "chatbubbles"],
      menuShow: false,
      page: {
        pageName: [
          "Cover",
          "Recipe",
          "HighFive",
          "Calendar",
          "Gallery",
          "AboutMe",
        ],
        pageSet: "",
      },
    };
  },
  methods: {
    menuActive() {
      this.menuShow = !this.menuShow;
      const menuL = document.querySelector("#headerMenuL");
      const menuR = document.querySelector("#headerMenuR");
      this.classSAH(menuL);
      this.classSAH(menuR);
      const cc = "close-circle";
      const hc = "heart-circle-outline";
      const startIcon = document.querySelector("#startIcon");
      this.menuShow ? (startIcon.name = cc) : (startIcon.name = hc);
      this.fade(this.menuShow);
    },
    iconHover(index, LR) {
      if (LR == "R") {
        index += 3;
      }
      const text = document.querySelectorAll(".menuText")[index];
      this.classSAH(text);
    },
    setPage(index, LR) {
      if (LR == "R") {
        index += 3;
      }
      this.iconHover(index);
      const page = this.page;
      page.pageSet = page.pageName[index];
      this.$emit("setPage", page.pageSet);
    },
    classSAH(e) {
      if (e.classList.contains("hide")) {
        e.classList.remove("hide");
        e.classList.add("show");
      } else {
        e.classList.remove("show");
        e.classList.add("hide");
      }
    },
    fade(TF) {
      const header = document.querySelector("#header");
      TF ? header.classList.remove("hide") : header.classList.add("hide");
      !TF ? header.classList.remove("show") : header.classList.add("show");
    },
  },
};
</script>
<template>
  <div id="header" class="hide">
    <div id="headerMenuL" class="hide" v-show="menuShow">
      <div id="menuL">
        <div class="headerBtn" v-for="(item, index) in menuL" :key="item">
          <ion-icon
            :name="iconL[index]"
            class="icon"
            @mouseenter="iconHover(index, 'L')"
            @mouseleave="iconHover(index, 'L')"
            @mouseup="iconHover(index, 'L')"
            @click="setPage(index, 'L')"
          ></ion-icon>
          <div class="menuText hide">{{ menuL[index] }}</div>
        </div>
      </div>
    </div>

    <div id="empty"></div>
    <div id="start">
      <div class="headerBtn">
        <ion-icon
          id="startIcon"
          name="heart-circle-outline"
          class="icon"
          @click="menuActive()"
        ></ion-icon>
      </div>
    </div>

    <div id="headerMenuR" class="hide" v-show="menuShow">
      <div id="menuR">
        <div class="headerBtn" v-for="(item, index) in menuR" :key="item">
          <ion-icon
            :name="iconR[index]"
            class="icon"
            @mouseenter="iconHover(index, 'R')"
            @mouseleave="iconHover(index, 'R')"
            @mouseup="iconHover(index, 'R')"
            @click="setPage(index, 'R')"
          ></ion-icon>
          <div class="menuText hide">{{ menuR[index] }}</div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
#header {
  font-family: var(--ff-zhaijia);
  height: var(--btn-size);
  color: var(--color-purple);
  border-radius: calc(var(--btn-size) / 2);
  background-color: var(--color-lightgrey);
  box-shadow: var(--shadow-purple);
  display: grid;
  grid-template-columns: calc(var(--btn-size) * 3) var(--btn-size) calc(
      var(--btn-size) * 3
    );
  justify-items: center;
  align-items: center;
  position: relative;
  transition: all 0.3s ease-in-out;
  overflow: hidden;
}
#header.hide {
  animation: headerHide 0.3s linear forwards, flowHide 0.1s 0.5s linear forwards;
}
@keyframes headerHide {
  100% {
    background-color: transparent;
    box-shadow: none;
    grid-template-columns: var(--btn-size);
  }
  90% {
    grid-template-columns: calc(var(--btn-size) * 1) var(--btn-size) calc(
        var(--btn-size) * 1
      );
  }
  55% {
    grid-template-columns: calc(var(--btn-size) * 2) var(--btn-size) calc(
        var(--btn-size) * 2
      );
  }
  0% {
    grid-template-columns: calc(var(--btn-size) * 3) var(--btn-size) calc(
        var(--btn-size) * 3
      );
  }
}
@keyframes flowHide {
  100% {
    overflow: hidden;
  }
  0% {
    overflow: visible;
  }
}
#header.show {
  animation: headerShow 0.5s linear forwards, flowShow 0.1s 0.5s linear forwards;
}
@keyframes headerShow {
  0% {
    background-color: transparent;
    box-shadow: none;
    grid-template-columns: var(--btn-size);
  }
  15% {
    grid-template-columns: calc(var(--btn-size) * 1) var(--btn-size) calc(
        var(--btn-size) * 1
      );
  }
  55% {
    grid-template-columns: calc(var(--btn-size) * 2) var(--btn-size) calc(
        var(--btn-size) * 2
      );
  }
  100% {
    grid-template-columns: calc(var(--btn-size) * 3) var(--btn-size) calc(
        var(--btn-size) * 3
      );
  }
}
@keyframes flowShow {
  0% {
    overflow: hidden;
  }
  100% {
    overflow: visible;
  }
}
#menuL,
#menuR {
  display: inline-grid;
  justify-items: center;
  align-items: center;
  grid-template-columns: repeat(3, var(--btn-size));
  grid-template-rows: 6vh;
}

#start {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  color: #fff;
}
.headerBtn {
  position: relative;
}
.icon {
  font-size: calc(var(--btn-size) / 2.2);
  cursor: pointer;
  text-shadow: var(--shadow-purple);
}
.menuText {
  min-width: var(--btn-size);
  max-height: calc(var(--btn-size) / 2);
  font-size: 12px;
  font-weight: 600;
  position: absolute;
  text-align: center;
  left: 50%;
  transform: translateX(-50%);
  top: calc(var(--btn-size) / 1.2);
  background-color: var(--color-lightgrey);
  border-radius: calc(var(--btn-size) / 2);
  opacity: 0;
}
.menuText.show {
  animation: menuTextShow 0.5s forwards;
}
.menuText.hide {
  animation: menuTextHide 0.5s forwards;
}
@keyframes menuTextShow {
  0% {
    top: calc(var(--btn-size) / 1.2);
    opacity: 0;
    line-height: calc(var(--btn-size) / 1);
    margin-top: 2vh;
  }
  100% {
    top: calc(var(--btn-size) / 1.5);
    opacity: 1;
    line-height: calc(var(--btn-size) / 2);
    margin-top: 4vh;
  }
}
@keyframes menuTextHide {
  0% {
    top: calc(var(--btn-size) / 1.5);
    line-height: calc(var(--btn-size) / 2);
    opacity: 1;
    margin-top: 4vh;
  }
  100% {
    top: calc(var(--btn-size) / 1.2);
    line-height: calc(var(--btn-size) / 1);
    opacity: 0;
    margin-top: 2vh;
  }
}
#footer {
  width: 100%;
  color: var(--color-lightgrey);
  font-size: small;
  text-align: center;
  position: fixed;
  bottom: 10px;
  white-space: pre-wrap;
}
</style>
