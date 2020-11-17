<template>
  <div id="app">
      <!-- <div> -->
    <div class="inputs-container">
      <div class="input-group mb-3 create-container">
        <div class="input-group-prepend">
          <button
            @click="addItem(title)"
            type="button" class="btn btn-success"
            id="button-addon1"
          >
            <img src="./assets/add.svg" alt="trash" width="20"
          >
          </button>
        </div>
        <input type="text" class="form-control" placeholder="" v-model="title">
      </div>
      <div class="input-group flex-nowrap filter">
        <div class="input-group-prepend">
          <button class="btn btn-dark" id="addon-wrapping">
            <img src="./assets/filter.svg" alt="filter" width="20">
          </button>
        </div>
        <input
          type="text"
          class="form-control"
          placeholder="Find todo"
          @change="getFilteredList(filter)"
          v-model="filter"
        >
      </div>
    </div>
    <ul class="list-container">
      <TodoList v-for="item in paginatedData" :key="item.id" :item="item" :show="selectedItemId === item.id"  @select="onSelect"/>
    </ul>
    <Pagination :pageNumber="pageNumber" @changePage="handleChangePage" :pageCount="pageCount"/>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';
import Pagination from './components/Pagination.vue';

export default {
  name: 'App',
  components: {
    TodoList,
    Pagination,
  },
  data() {
    return {
      items: [],
      selectedItemId: null,
      pageNumber: 0,
    }
  },
  async mounted() {
    const API_URL = `http://localhost:3000/todos`;
    this.items = await fetch(API_URL).then((res) => res.json());
  },

  computed: {
    pageCount() {
      return Math.ceil(this.items.length/5);
    },
    itemsCount() {
      return this.filteredItems.length;
    },
    paginatedData() {
      const start = this.pageNumber * 5;
      const end = start + 5;
      return this.items.slice(start, end);
    }
  },

  methods: {
    onSelect(data){
      this.selectedItemId = this.selectedItemId === data.selectedId ? null : data.selectedId;
    },
    handleChangePage(data){
      if(data.mode === 'next'){
        this.pageNumber++;
      }
      if(data.mode === 'prev'){
        this.pageNumber--;
      }
    },
    getFilteredList(filter){
      const filteredList = this.items.filter(item => {
        return item.title.includes(filter)
      })
      this.getPageList(filteredList);
    },

    selectItem(index) {
      this.selectedIndex = this.selectedIndex === index ? null : index;
    },

    removeItem(item) {
      fetch(`http://localhost:3000/todos/${item.id}`, {
      method: 'DELETE',
    })
      this.items = this.items.filter((x) => x !== item);
      this.getPageList(this.items);
    },

    updateItem(item) {
      fetch(`http://localhost:3000/todos/${item.id}`, {
      method: 'PUT',
      body: JSON.stringify(item),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    })
      .then((response) => response.json())
      .then((json) => console.log(json))
    },

    addItem(title){
      const item = {
        id: Math.floor(Math.random() * 1000),
        title,
      }
      fetch('http://localhost:3000/todos', {
      method: 'POST',
      body: JSON.stringify(item),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    })
      .then((response) => response.json())
      .then((json) => console.log(json))
      this.items.push(item);

      this.title = '';
    },
  },
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
  }
  .list-container{
    width: 80%;
    min-height: 300px;
  }
</style>
