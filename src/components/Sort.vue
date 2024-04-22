<template>
  <div class="sort">
    <v-autocomplete
        v-model="filters.sortBy"
        label="Сортировка"
        :items="['без сортировки', 'по возрастанию рейтинга', 'по убыванию рейтинга']"
        variant="outlined"
    ></v-autocomplete>
  </div>
</template>

<script setup>
import {inject, onMounted, reactive, ref, watch} from "vue";

const firstList = inject('firstList');
const secondList = inject('secondList');
const lastList = inject('lastList');

const props = defineProps({
  options: {},
});

let cards = ref([]);

const filters = reactive({
  sortBy: 'без сортировки',
})

function getLocalCards() {
  switch (props.options.id) {
    case 1:
      cards = firstList;
      break;
    case 2:
      cards = secondList;
      break;
    case 3:
      cards = lastList;
      break;
  }
}

function sortList() {
  getLocalCards();
  switch (filters.sortBy) {
    case 'без сортировки':
      cards = cards.value.sort((a, b) => a.id - b.id);
      break;
    case 'по возрастанию рейтинга':
      cards = cards.value.sort((a, b) => a.rating.rate - b.rating.rate);
      break;
    case 'по убыванию рейтинга':
      cards = cards.value.sort((a, b) => b.rating.rate - a.rating.rate);
      break;
  }
}

onMounted(async () => {
  await sortList();
})

watch(filters, sortList);
</script>

<style scoped lang="scss">
.sort {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 400px;
  color: #FFF;
}
</style>