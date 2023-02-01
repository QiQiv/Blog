<script>
import Header from "./components/Header.vue";
import Cover from "./components/Cover.vue";
import Recipe from "./components/Recipe.vue";
import Calendar from "./components/Calendar.vue";
import HighFive from "./components/HighFive.vue";
import AboutMe from "./components/AboutMe.vue";
import Gallery from "./components/Gallery.vue";

export default {
  components: {
    Header,
    Cover,
    Recipe,
    Calendar,
    HighFive,
    Gallery,
    AboutMe,
  },
  data() {
    return {
      page: "Cover",
    };
  },
  methods: {
    setPage(value) {
      this.page = value;
    },
  },
};
</script>

<template>
  <div>
    <Header id="header" @setPage="setPage"></Header>
    <component :is="page" class="cpn"></component>
  </div>
</template>

<style scoped>
#header {
  width: max-content;
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  top: 0;
  z-index: 99;
}
.cpn {
  width: 100vw;
  height: 100vh;
}
</style>
