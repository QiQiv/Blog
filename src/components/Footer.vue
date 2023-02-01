<script>
export default {
  data() {
    return {
      disclaimer:
        "博客部分图片收集于网络，仅供个人学习和欣赏使用。\n版权归原作者（含AI）所有。\n如侵犯了您的版权，请联系我删除并向您致歉。",
    };
  },
};
</script>
<template>
  <div id="box">
    <div id="footer">
      {{ disclaimer }}
    </div>
  </div>
</template>
<style scoped>
#footer {
  width: 100%;
  color: var(--color-lightgrey);
  scale: 0.6;
  text-align: center;
  position: fixed;
  bottom: 10px;
  white-space: pre-wrap;
}
</style>
