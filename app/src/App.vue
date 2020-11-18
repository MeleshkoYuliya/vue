<template>
  <div id="app">
    <div class="inputs-container">
      <CreateItemInput @addItem="handleAddItem"/>
      <FilterInput v-model="query"/>
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
import FilterInput from './components/Filter';
import CreateItemInput from './components/CreateItemInput';

export default {
  name: 'App',
  components: {
    TodoList,
    Pagination,
    FilterInput,
    CreateItemInput,
  },
  data() {
    return {
      items: [],
      selectedItemId: null,
      pageNumber: 0,
      title: '',
      query: '',
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
      if(!this.query){
        return this.items.slice(start, end);
      } else {
        const filteredItems = this.items.filter((item) => item.title.toLowerCase().includes(this.query.toLowerCase()));
        return filteredItems.slice(start, end);
      }
    }
  },

  methods: {
    handleAddItem(data){
      const item = {
        id: Math.floor(Math.random() * 1000),
        title: data,
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
    handleSearch(data){
      this.query = data;
    },
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
