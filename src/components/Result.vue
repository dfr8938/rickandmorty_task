<template>
  <div :class="nextList.length === 0 ? 'nonresults' : 'results'">
    <h4>Найдено совпадений:</h4> {{countCharacter}}
  </div>
</template>

<script>
  export default {
      props: {
        nextList: {
          type: Array,
          required: true
        },
        countCharacter: {
          type: Number,
          required: true
        }
      }
  }
</script>

<style scoped>
  .results {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100px;
    color: #ffffff;
  }

  .results h4 {
    font-weight: bold;
    color: #ffffff;
    padding: 0 10px;
  }

  .nonresults {
    display: none;
    width: 100%;
    height: 30px;
    color: #ffffff;
  }
</style>