<template>
  <div class="box">
    <div class="app">
      <div class="item" v-for="item in nextList.length > 0 ? nextList : list" :key="item">
        <div class="item-img">
          <img :src="item.image" alt="">
        </div>
        <div class="item-info">
          <div class="section">
            <h2>{{item.name}}</h2>
            <div class="status">
              {{item.status}} - {{item.species}}
            </div>
          </div>
          <div class="section">
            <span class="title">Last known location:</span>
            <div class="episode">{{item.origin.name}}</div>
          </div>
          <div class="section">
            <span class="title">First seen in:</span>
            <div class="location">{{item.location.name}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      nextList: {
        type: Array,
        required: true
      },
      list: {
        type: Array,
        required: true
      },
    }
  }
</script>

<style scoped>
  .box {
    display: flex;
    -webkit-box-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    align-items: center;
    padding: 4.5rem 0px;
    background: rgb(39, 43, 51);
    min-height: calc(-60px + 50vh);
  }

  .app {
    display: flex;
    -webkit-box-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    align-items: center;
    flex-wrap: wrap;
    max-width: 1920px;
  }

  .item {
    width: 600px;
    height: 220px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgb(60, 62, 68);
    border-radius: 0.5rem;
    margin: 0.75rem;
    overflow: hidden;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
  }

  .item-img {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 229px;
    height: 220px;
    overflow: hidden;
  }

  .item-img img {
    width: 100%;
    height: 100%;
  }

  .item-info {
    flex: 3 1 0%;
    position: relative;
    padding: 0.75rem;
    color: rgb(255, 255, 255);
    display: flex;
    flex-direction: column;
  }

  .section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    width: 100%;
    height: 76px;
  }

  .section h2 {
    cursor: pointer;
    font-size: 1.5rem;
    margin-top: 1.625rem;
    padding-bottom: calc(0.40625rem - 1px);
    margin-bottom: 1px;
    color: rgb(255, 255, 255);
    font-family: -apple-system, 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'Helvetica', 'Arial', sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
    font-weight: 800;
    text-rendering: optimizeLegibility;
  }

  .section h2:hover {
    color: rgb(255, 152, 0);;
  }

  .status {
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    text-transform: capitalize;
  }

  .status {
    font-size: 16px;
    font-weight: 500;
    color: rgb(255, 255, 255);
  }

  .title {
    color: #9e9e9e;
    padding-bottom: 10px;
  }

  .location, .episode {
    cursor: pointer;
    font-size: 18px;
    color: #f5f5f5;
  }

  .location:hover, .episode:hover {
    color: rgb(255, 152, 0);
  }
</style>