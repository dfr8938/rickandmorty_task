<template>
  <div class="pages">
    <button
        class="prev"
        @click="prevPage"><<</button>
    <div
        v-for="pageNum in countPages"
        :key="pageNum"
        :class="{
          'current_page': page === pageNum,
        }"
        @click="changePage(pageNum)"
    >
      <div class="page">{{pageNum}}</div>
    </div>
    <button
        class="next"
        @click="nextPage">>></button>
  </div>
</template>

<script>
  export default {
    props: {
      page: {
        type: Number,
        required: true
      },
      countPages: {
        type: Number,
        required: true
      },
      prevPage: {
        type: Function,
        required: true
      },
      changePage: {
        type: Function,
        required: true
      },
      nextPage: {
        type: Function,
        required: true
      }
    }
  }
</script>

<style scoped>
  .page, .prev, .next {
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    height: 30px;
    margin: 2px;
    padding: 5px;
    color: #ffffff;
    border-radius: 3px;
    background: #9e9e9e;
  }

  .pages {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    padding: 10px;
  }

  .prev, .next {
    border: none;
  }

  .current_page {
    background: #aaaaaa;
  }
</style>