<script setup>
import { ref, onMounted, nextTick } from 'vue';
const WebLinks = ref([]);
const WebLink = ref('');
const WebNames = ref([]);
const WebName = ref('');

onMounted(() => {
  const storage = localStorage.getItem('auo-link');

  if (storage != null) {
    const result = JSON.parse(storage);
    WebLinks.value = result;
  }
});

const clearLink = () => {
  WebLinks.value = [];
  localStorage.removeItem('auo-link');
};

const addLink = () => {
  if (WebLink.value != '') {
    const Link = {
      id: crypto.randomUUID(),
      name: WebName.value,
      link: WebLink.value,
    };

    WebLinks.value.unshift(Link);

    localStorage.setItem('auo-link', JSON.stringify(WebLinks.value));
    WebName.value = '';
    WebLink.value = '';
  }
};

const OpenNewTab = (link) => {
  console.log('Opening URL:', link);
  window.open(link, '_blank');
};

async function removeLink(x) {
  console.log(x);
  const all_row = JSON.parse(localStorage.getItem('auo-link')) || [];
  const index = all_row.findIndex((item) => item.id === x);
  all_row.splice(index, 1);
  localStorage.setItem('auo-link', JSON.stringify(all_row));

  const storage = localStorage.getItem('auo-link');

  if (storage != null) {
    const result = JSON.parse(storage);
    WebLinks.value = result;
  }
}
</script>

<template>
  <h1>收藏網址⼩⼯具</h1>
  <h1>Web Name</h1>
  <input v-model.trim="WebName" type="text" />
  <h1>Web Link</h1>
  <input @keyup.enter="addLink" v-model.trim="WebLink" type="text" />
  <button @click="addLink">新增</button>
  <button @click="clearLink">清除</button>
  <hr />
  <ul>
    <li v-for="WebLink in WebLinks" :key="WebLink.id">
      {{ WebLink.name }} {{ WebLink.link }}
      <button @click="OpenNewTab(WebLink.link)">開啟網址</button>
      <button @click="removeLink(WebLink.id)">Remove</button>
    </li>
  </ul>
</template>

<style scoped></style>
