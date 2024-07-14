<template>
  <div class="search">
    <label>Поиск по имени:
    <input
        v-model="searchQuery"
        type="text"
        placeholder="введите текст поиска"/>
    <button @click="pastArrayList">Найти</button>
    </label>
  </div>
  <div :class="nextList.length === 0 ? 'nonresults' : 'results'">
    <h4>Найдено совпадений:</h4> {{nextList.length}}
  </div>
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
  <div class="pages">
    <button
        :class="countPages === 0 ? 'nonprev' : 'prev'"
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
        :class="countPages === 0 ? 'nonnext' : 'next'"
        @click="nextPage">>></button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      list: [],
      pastList: [],
      nextList: [],
      searchArray: [],
      page: 1,
      countCharacter: 0,
      countPages: 0,
      searchQuery: "",
    }
  },
  beforeMount() {

  },
  mounted() {
    this.fetchCards();
    this.pageCount();
    this.characterCount();
  },
  computed: {

  },
  methods: {
    async fetchCards() {
      await axios.get(`https://rickandmortyapi.com/api/character/?page=${this.page}`)
          .then(res => this.list = res.data.results);
    },
    async pastArrayList() {
      try {
        await axios.get(`https://rickandmortyapi.com/api/character/?page=${this.page}&name=${this.searchQuery}&status=alive`)
            .then(res => this.pastList = res.data.info);
        await axios.get(`https://rickandmortyapi.com/api/character/?page=${this.page}&name=${this.searchQuery}&status=alive`)
            .then(res => this.nextList = res.data.results);
        this.countPages = this.pastList.pages;
        this.countCharacter = this.pastList.count;
      } catch (e) {
        console.error(e.message);
      }
    },
    async pageCount() {
      await axios.get(`https://rickandmortyapi.com/api/character`)
          .then(res => this.countPages = res.data.info.pages);
    },
    async characterCount() {
      await axios.get(`https://rickandmortyapi.com/api/character`)
          .then(res => this.countCharacter = res.data.info.count);
    },
    prevPage() {
      try {
        if (this.page > 1 && this.page <= this.countPages) {
          this.page -= 1;
        } else if (this.page === 0) {
          this.page = this.countPages;
        } else {
          this.page = (this.countPages + 1) - 1;
        }
        this.nextList.length > 0 ? this.pastArrayList() : this.fetchCards();
      } catch (e) {
        console.error(e.message);
      }
    },
    changePage(page) {
      this.page = page;
      this.nextList.length > 0 ? this.pastArrayList() : this.fetchCards();
    },
    nextPage() {
      try {
        if (this.page > 0 && this.page < this.countPages) {
          this.page += 1;
        } else {
          this.page = 1;
        }
        this.nextList.length > 0 ? this.pastArrayList() : this.fetchCards();
      } catch (e) {
      }
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    color: hsla(0, 0%, 0%, 0.8);
    font-family: -apple-system, 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'Helvetica', 'Arial', sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
    font-weight: normal;
    word-wrap: break-word;
    font-kerning: normal;
  }

  body {
    background: rgb(39, 43, 51);
  }

  .search {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 50px;
    padding-top: 70px;
  }

  .search input {
    text-indent: 10px;
    width: 30%;
    height: 30px;
    border: none;
    border-radius: 10px 0 0 10px;
    outline: none;
  }

  .search button {
    padding: 5px 10px;
    height: 30px;
    border: none;
    outline: none;
    color: #ffffff;
    border-radius: 0 10px 10px 0;
    transition: .5s ease all;
    background-color: #cccccc;
  }

  .search button:hover {
    background-color: rgb(39, 43, 51);
    transition: .5s ease all;
  }

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

  .pages {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    padding: 10px;
  }

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

  .prev, .next {
    border: none;
  }

  .nonnext, .nonprev {
    display: none;
  }

  .current_page {
    background: #aaaaaa;
  }

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
