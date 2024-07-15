<template>
  <div class="search">
    <input
        v-model="searchQuery"
        type="text"
        placeholder="Введите имя персонажа"/>
    <select class="select_status" v-model="selected">
      <option disabled value="">Выберите статус</option>
      <option>Alive</option>
      <option>Dead</option>
      <option>Unknown</option>
    </select>
    <button @click="pastArrayList">Применить</button>
  </div>
  <Result
      :countCharacter="countCharacter"
      :nextList="nextList" />
  <List
      :list="list"
      :nextList="nextList" />
  <Pagination
      :page="page"
      :nextPage="nextPage"
      :changePage="changePage"
      :prevPage="prevPage"
      :countPages="countPages" />
</template>

<script>
import axios from "axios";
import {ref} from "vue";
import { BASIC_URl } from "./util/consts.js";
import Result from "./components/Result.vue";
import List from "./components/List.vue";
import Pagination from "./components/Pagination.vue";

export default {
  components: {Pagination, List, Result},
  data() {
    return {
      list:[],
      pastList: [],
      nextList: [],
      searchArray: [],
      page: ref(1),
      countCharacter: ref(0),
      countPages: ref(0),
      searchQuery: ref(""),
      selected: ref(""),
      notfound: ref(""),
    }
  },
  mounted() {
    this.fetchCards();
    this.pageCount();
    this.characterCount();
  },
  methods: {
    async fetchCards() {
      await axios.get(`${BASIC_URl}/?page=${this.page}`)
          .then(res => this.list = res.data.results);
    },
    async pastArrayList() {
      try {
        this.nextList = [];
        if (this.selected === "") {
          this.selected = "Alive";
        }
        await axios.get(`${BASIC_URl}/?page=${this.page}&name=${this.searchQuery}&status=${this.selected}`)
            .then(res => this.pastList = res.data.info);
        await axios.get(`${BASIC_URl}/?page=${this.page}&name=${this.searchQuery}&status=${this.selected}`)
            .then(res => this.nextList = res.data.results);
        this.countPages = this.pastList.pages;
        this.countCharacter = this.pastList.count;
      } catch (e) {
          if (e.response && e.response.status === 404) {
            console.warn("404 - Данные отсутствуют!");
          }
      }
    },
    async pageCount() {
      await axios.get(`${BASIC_URl}`)
          .then(res => this.countPages = res.data.info.pages);
    },
    async characterCount() {
      await axios.get(`${BASIC_URl}`)
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
    width: 170px;
    height: 30px;
    border: none;
    border-radius: 10px 0 0 10px;
    outline: none;
  }

  .select_status {
    width: 130px;
    height: 30px;
    border: none;
    outline: none;
    color: #302e2c;
    border-left: 1px solid #eeeeee;
    background-color: #ffffff;
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
</style>
