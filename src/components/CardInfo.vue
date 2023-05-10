<script setup>
import { ref } from 'vue';

const props = defineProps({
  postCardInfo: {
    type: Object,
    required: false,
  },
});
</script>
<template>
  <div class="cardInfo">
    <div class="cardImg">
      <img :src="postCardInfo?.sprites?.front_default" />
      <h1>{{ props.postCardInfo.name }}</h1>
    </div>
    <div class="cardContent">
      <h2 v-for="item of props.postCardInfo.stats" :key="item.index">{{ item.stat.name }}:{{ item.base_stat }}</h2>
    </div>
  </div>
</template>
<style scoped>
.cardInfo {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 250px;
}
.cardImg {
  width: 20%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.cardContent {
  width: 20%;
}
@media screen and (max-width: 900px) {
  .cardImg,
  .cardContent {
    width: 50%;
  }
}
</style>
