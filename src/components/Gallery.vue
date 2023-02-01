<script>
let checkBox = null;
export default {
  data() {
    return {
      imgUrl: [
        "gallery-1.jpg",
        "gallery-2.jpg",
        "gallery-3.jpg",
        "gallery-4.jpg",
        "gallery-5.jpg",
      ],
      imgTitle: ["原神", "神里凌华", "温迪", "七七", "心海"],
    };
  },
  methods: {
    cardHover(e) {
      const img = e.currentTarget;
      img.classList.toggle("filter");
    },
    checkImg(e) {
      if (checkBox === null) {
        checkBox = document.createElement("div");
        checkBox.setAttribute("id", "checkBox");

        const img = document.createElement("img");
        img.setAttribute("id", "checkImg");
        const url = e.currentTarget.src;
        img.src = url;
        checkBox.appendChild(img);

        const text = document.createElement("h2");
        text.innerText = e.currentTarget.title;
        text.setAttribute("id", "checkText");
        checkBox.appendChild(text);

        checkBox.addEventListener("click", this.closeImg);
        const parent = document.querySelector("#box");
        parent.insertBefore(checkBox, parent.childNodes[0]);
      }
    },
    closeImg() {
      if (checkBox !== null) {
        const img = document.querySelector("#checkBox");
        img.remove();
        checkBox = null;
      }
    },
  },
  setup() {
    const getUrl = (name) => {
      return new URL(`../assets/images/${name}`, import.meta.url).href; //本地文件路径
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
      <div id="gallery">
        <div class="card" v-for="(url, index) in imgUrl" :key="index">
          <img
            :src="getUrl(imgUrl[index])"
            :title="imgTitle[index]"
            class="img filter"
            @mouseenter="cardHover($event)"
            @mouseleave="cardHover($event)"
            @mouseup="cardHover($event)"
            @click="cardHover($event), checkImg($event)"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
#box {
  width: 100vw;
  height: 100vh;
  background-image: url("../assets/images/gallery-bg.jpg");
  background-size: cover;
  background-position: center;
}
#empty {
  height: var(--btn-size);
}
#container {
  width: 100%;
  height: calc(100vh - var(--btn-size));
}
#gallery {
  width: 90%;
  height: 80%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(5, 1fr);
  grid-column-gap: 10px;
  grid-row-gap: 10px;
  border-radius: 5px;
  overflow: hidden;
  position: relative;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  background-color: var(--color-lightgrey);
  border: var(--color-lightgrey) 10px solid;
}
.card:nth-child(1) {
  grid-area: 1 / 1 / 3 / 3;
}
.card:nth-child(2) {
  grid-area: 1 / 3 / 4 / 4;
}
.card:nth-child(3) {
  grid-area: 3 / 1 / 6 / 2;
}
.card:nth-child(4) {
  grid-area: 3 / 2 / 4 / 3;
}
.card:nth-child(5) {
  grid-area: 4 / 2 / 6 / 4;
}
.img {
  height: 100%;
  width: 100%;
  object-fit: cover;
  transition: all 0.3s ease-in-out;
  cursor: pointer;
}
.filter {
  filter: saturate(120%) contrast(80%) blur(1px);
  transition: all 0.2s ease-in-out;
}
</style>
<style>
#checkBox {
  width: 100vw;
  height: 100vh;
  position: fixed;
  z-index: 1;
  background-color: rgba(0, 0, 0, 0.8);
  color: #fff;
  font-family: var(--ff-hutu);
}
#checkImg,
#checkText {
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
}
#checkImg {
  width: 80%;
  border-radius: 5px;
  border: var(--color-lightgrey) 10px solid;
}
</style>
