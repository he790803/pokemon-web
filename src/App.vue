<script setup>
import { ref, reactive, watchEffect, onMounted } from 'vue';
import axios from 'axios';
import Card from './components/Card.vue';
import CardInfo from './components/CardInfo.vue';

const data = ref({});
const pokeData = reactive([]); // 使用 reactive() 來定義一個可以自動追蹤變化的 state 變數
const url = ref('https://pokeapi.co/api/v2/pokemon/?limit=12');
const loading = ref(true);
const nextUrl = ref(null);
const prevUrl = ref(null);

const getCardInfo = (res) => {
  data.value = res;
};

const nextHandler = () => {
  if (nextUrl.value !== null) {
    pokeData.length = 0;
    url.value = nextUrl.value;
    getPokemonData();
  }
};

const prevHandler = () => {
  if (prevUrl.value !== null) {
    pokeData.length = 0;
    url.value = prevUrl.value;
    getPokemonData();
  }
};

const getPokemon = async (res) => {
  const promises = res.map(async (item) => {
    const result = await axios.get(item.url);
    return result.data;
  });
  const values = await Promise.all(promises); //Promise.all等待所有promises完成
  pokeData.push(...values);
  pokeData.sort((a, b) => (a.id > b.id ? 1 : -1));
  loading.value = false;

  // for (const item of res) {
  //   // 使用 for...of 迴圈來處理陣列中的每一個元素
  //   const result = await axios.get(item.url);
  //   pokeData.push(result.data); // 直接將結果 push 到 reactive 變數中即可
  // }
  // pokeData.sort((a, b) => (a.id > b.id ? 1 : -1)); // 在結束迴圈之後，對 pokeData 進行排序
  // loading.value = false; // 設定 loading 為 false，隱藏載入中的畫面
};

const getPokemonData = async () => {
  loading.value = true;
  try {
    const response = await axios.get(url.value);
    const res = response.data;
    nextUrl.value = res.next;
    prevUrl.value = res.previous;
    getPokemon(res.results);
  } catch {
    console.log('Error:', error.message);
  }
};

onMounted(() => {
  getPokemonData();
});
</script>

<template>
  <div class="main">
    <header>
      <h1>寶可夢資料查詢</h1>
    </header>
    <template v-if="loading">
      <div class="loading">
        <h1>Loading...</h1>
      </div>
    </template>

    <template v-else>
      <CardInfo :postCardInfo="data" />
      <Card :data="pokeData" @cardInfo="getCardInfo" />
      <div class="buttonGroup">
        <button @click="prevHandler">上一頁</button>
        <button @click="nextHandler">下一頁</button>
      </div>
    </template>
  </div>
</template>

<style scoped>
.main {
  height: 100%;
}
header {
  background-color: crimson;
  height: 100px;
}
header h1 {
  text-align: center;
  line-height: 100px;
  color: aliceblue;
}
.buttonGroup {
  display: flex;
  justify-content: center;
}
button {
  width: 120px;
  height: 50px;
  border: 0;
  margin: 1rem;
  box-shadow: 5px 5px 0 rgba(0, 0, 0, 0.3);
  font-weight: bolder;
  background-color: crimson;
  color: azure;
}
button:active {
  transform: translateY(2px);
  box-shadow: 0px 0px 0 rgba(0, 0, 0, 0.3);
}
.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  height: calc(100vh - 100px);
  position: relative;
  top: 0;
  z-index: 10;
}
@media screen and (max-width: 900px) {
  header {
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 10;
    height: 100px;
  }
  .buttonGroup {
    position: relative;
    bottom: -350px;
    z-index: 10;
  }
}
</style>
