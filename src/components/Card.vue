<script setup>
import { ref } from 'vue';

const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});
const emit = defineEmits(['cardInfo']);
const cardInfo = (item) => {
  emit('cardInfo', item);
};
// console.log(props.data);
</script>
<template>
  <div class="cardGroup">
    <div
      class="card"
      v-for="item of props.data"
      :class="{ [item.types[0].type.name]: true }"
      :key="item.name"
      @click="cardInfo(item)"
    >
      <img :src="item.sprites.front_default" />
      <h1>{{ item.name }}</h1>
    </div>
  </div>
</template>
<style scoped>
.cardGroup {
  display: flex;
  flex-wrap: wrap;
  height: 70%;
  /* position: relative;
  top: 0px; */
  justify-content: center;
}
.card {
  width: 23%;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1rem;
  border-radius: 10px;
  box-shadow: 5px 5px 0 rgba(0, 0, 0, 0.3);
}
.card:active {
  transform: translateY(2px);
  box-shadow: 0px 0px 0 rgba(0, 0, 0, 0.3);
}
.normal {
  background-color: blanchedalmond;
}
.fighting {
  background-color: coral;
}
.flying {
  background-color: darkturquoise;
}
.poison {
  background-color: blueviolet;
}
.ground {
  background-color: chocolate;
}
.rock {
  background-color: darkgoldenrod;
}
.bug {
  background-color: chartreuse;
}
.ghost {
  background-color: darkgray;
}
.steel {
  background-color: dimgray;
}
.fire {
  background-color: darkorange;
}
.water {
  background-color: deepskyblue;
}
.grass {
  background-color: cadetblue;
}
.electric {
  background-color: goldenrod;
}

@media screen and (max-width: 900px) {
  .cardGroup {
    position: relative;
    top: 350px;
  }
  .card {
    width: 40%;
  }
}
@media screen and (max-width: 700px) {
  .card {
    width: 100%;
  }
}
</style>
