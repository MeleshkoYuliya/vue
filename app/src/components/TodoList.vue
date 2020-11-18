<template>
  <li class="list-group-item" @click="selectItem">
    <span>{{item.title}}</span>
    <div v-show="show" class="btn-container">
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
</template>

<script>

export default {
  name: "todoList",
  props: ['item', 'show', 'index'],
  methods: {
  selectItem() {
    this.$emit('select', {selectedId: this.item.id});
  }
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
  .list-group-item{
    cursor: pointer;
  }
  .inputs-container{
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 80%;
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
