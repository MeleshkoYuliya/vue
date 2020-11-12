<template>
  <div>
    <div class="inputs-container">
      <div class="input-group mb-3 create-container">
        <div class="input-group-prepend">
          <button
            @click="addItem(title)"
            type="button" class="btn btn-success"
            id="button-addon1"
          >
            <img src="../assets/add.svg" alt="trash" width="20"
          >
          </button>
        </div>
        <input type="text" class="form-control" placeholder="" v-model="title">
      </div>
      <div class="input-group flex-nowrap filter">
        <div class="input-group-prepend">
          <button class="btn btn-dark" id="addon-wrapping">
            <img src="../assets/filter.svg" alt="filter" width="20">
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
      <li v-for="(item, index) in paginatedData" :key="item.id" @click="selectItem(index)" class="list-group-item">
        <span>{{item.title}}</span>
        <div v-show="selectedIndex===index" class="btn-container">
          <button 
            @click="removeItem(item)"
            type="button" class="btn btn-danger"
          >
            <img src="../assets/trash.svg" alt="trash" width="20">
          </button>
          <div class="input-group mb-3 input-container">
            <input
              type="text"
              class="form-control"
              placeholder="Recipient's username"
              v-model="item.title"
            >
            <div class="input-group-append">
              <button
                @click="updateItem(item)"
                class="btn btn-warning"
                type="button"
                id="button-addon2"
              >
                <img src="../assets/edit.svg" alt="edit" width="20">
              </button>
            </div>
          </div>
        </div>
      </li>
    </ul>
    <nav>
      <ul class="pagination">
        <li class="page-item">
          <button
            class="page-link"
            :disabled="pageNumber === 0" 
            @click="prevPage"
          >
            Previous
          </button>
        </li>
        <div class="page-link page-number">{{pageNumber + 1}}</div>
        <li class="page-item">
          <button 
            class="page-link" 
            :disabled="pageNumber >= pageCount -1" 
            @click="nextPage">
            Next
          </button>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
const API_URL = `http://localhost:3000/todos`;
export default {
  name: "todoList",
  data() {
    return {
      items: [],
      selectedIndex: null,
      updatingIndex: null,
      title:'',
      pageNumber: 0,
      paginatedData: [],
      pageCount: 0,
      filter: ''
    }
  },

  methods: {
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
      this.pageCount = Math.ceil(this.items.length/5);
      this.getPageList(this.items);
      this.title = '';
    },

    nextPage(){
      this.pageNumber++;
      this.getPageList(this.items)
    },

    prevPage(){
      this.pageNumber--;
      this.getPageList(this.items);
    },

    getPageList(data){
      const start = this.pageNumber * 5;
      const end = start + 5;
      this.paginatedData = data.slice(start, end);
    }
  },
  mounted() {
    fetch(API_URL)
        .then((res) => res.json())
        .then((data) => {
          this.items = data;
          this.pageCount = Math.ceil(data.length/5);
          this.getPageList(data);
        })
  }
}
</script>

<style scoped>
  .btn-container{
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    width: 400px;
    margin: 20px 0px 0px 0px;
  }
  .input-container{
    width: 300px
  }
  .create-container{
    width: 40%;
    margin: 15px 0px 0px 40px
  }
  .list-container{
    width: 80%;
    min-height: 300px;
  }
  .list-group-item{
    cursor: pointer;
  }
  .inputs-container{
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 80%;
  }
  .filter{
    width: 40%
  }
  .pagination{
    margin: 45px 0px 0px 40px;
  }
  .page-number{
    margin: 0px 10px;
    color:rgb(30, 30, 102);
    font-weight: 900;
  }
</style>
